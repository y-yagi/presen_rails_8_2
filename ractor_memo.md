# Rails 8.2 の Ractor 対応(調査メモ)

## 概要

* Rails アプリケーションを Ractor 上で動かすための対応が、2026年5月末〜9月にかけて集中的に行われている
  * コミットメッセージに "ractor" を含む非マージコミットは 2026/03 以降で約160件
  * 主な担い手は Shopify のメンバー(Gannon McGibbon / Andrew Novoselac / Étienne Barrié / Edouard CHIN)
  * フレームワーク別の変更数: Active Support 48 / Active Record 38 / railties 31 / Action Pack 29 / Action View 28 / Active Model 12 / その他少数
* まだ experimental。`ractorize!` を呼ぶと "Ractor support in Rails is experimental and subject to change." という警告が出る
* 前提は Ruby 4.0 以上(`Ractor.shareable_proc` / `Ractor::Port` などを使用)。4.0未満ではシムが何もしない実装になる
* eager load される環境(production)のみ対象。autoloader / reloader は `ractorize!` 時に捨てられる
* 動作には rack 側の対応(`rack/ractorize`)と `ractor-dispatch` gem が必要

## 基本的な考え方

Ractor 間で共有できるのは shareable なオブジェクト(deep freeze されたもの、クラス/モジュールなど)だけ。
Rails はクラスレベルの状態(設定値、キャッシュ、コールバック用の Proc など)を大量に持っているので、以下のいずれかで対応している。

1. boot 完了時に freeze / `Ractor.make_shareable` して共有可能にする
2. 遅延初期化(メモ化)していた値を boot 時に事前計算してから freeze する
3. 可変なキャッシュは Ractor ローカルストレージに移す
4. どうしても main Ractor でしか扱えない処理(DB 接続など)は main Ractor に処理を委譲する

## 1. 基盤: `ActiveSupport::Ractors`

`activesupport/lib/active_support/ractors.rb`(nodoc。フレームワーク内部用)

* shareability 系メソッドのシム(c941db66ab)
  * `make_shareable` / `shareable?` / `shareable_proc` / `shareable_lambda`
  * Ruby 4.0 以上では `Ractor.*` に委譲、それ未満では引数をそのまま返す。フレームワーク側はバージョン分岐を書かずに呼べる
  * 当初 Kernel にヘルパーを生やす案だったが revert され(5b20d2325b)、`ActiveSupport::Ractors` にまとめられた
* `try_shareable_proc` / `try_shareable_lambda` / `try_make_shareable`
  * shareable にしようとして `Ractor::IsolationError` になった場合の挙動を `ActiveSupport::Ractors.unshareable_proc_action` で制御(74015db85a)
    * `:raise` ... そのままエラー
    * `:warn` ... deprecation warning を出して元の(共有不可な)オブジェクトを返す
    * `nil`(デフォルト) ... 何もしない。既存アプリの挙動は変わらない
  * 背景: ユーザーのコード中の Proc が外側のローカル変数をキャプチャしていると shareable にできない

```ruby
Rails.application.routes.draw do
  to_resolve = [:basket, anchor: "items"]
  resolve("Cart") { to_resolve } # このブロックは shareable にできない
end
```

* `on_main` / `main?`(38ca8309a1)
  * `on_main` は非 main Ractor から main Ractor に処理を委譲する。内部で `ractor-dispatch` gem を使用
  * activesupport.gemspec に `ractor-dispatch >= 0.3.0` が依存として追加された
* `ActiveSupport::Ractors[]` / `[]=` / `store_if_absent`(444c16102d)
  * Ractor ローカルストレージのラッパー。キャッシュを Ractor ごとに持たせるのに使う
* `ActiveSupport::Ractors::Logger`
  * Ractor 間で共有可能な Logger。書き込みはバックグラウンドの Writer に `Ractor::Port` 経由で送る
  * 最初は `ActiveSupport::TaggedLogging.shareable_logger`(443e55dca2)として入り、その後この形に整理された

## 2. アプリを Ractor 用に準備する: `Rails::Application#ractorize!`

`railties/lib/rails/application.rb`(4ffef4ba58 で導入。nodoc)

boot 後に呼ぶと、アプリ全体を shareable にする。現在やっていること:

* `env_config` / `revision` / `routes` を事前計算
* autoloader / reloader / routes reloader を破棄
* view path、`ActiveSupport::TimeZone` を shareable に
* 全コントローラの `config` と `_wrapper_options` を shareable に
* Active Record 全モデルの reflection を shareable に
* Active Job 全ジョブの `queue_adapter` を shareable に
* `Rails.application` / `Rails.env` / `Rails.logger` / `Rails.event` / `Rails.error` / `Rails.backtrace_cleaner` を shareable に
* view テンプレートを全て eager load & コンパイル(a39d824f75)。ワーカー Ractor はコンパイル済みメソッドを呼ぶだけにする
* `rack/ractorize` を require(無ければエラー)

railties のテスト(`railties/test/application/ractors_test.rb`)での production 設定例:

```ruby
ActiveSupport::Ractors.unshareable_proc_action = :warn
config.logger = ActiveSupport::Ractors::Logger.new
config.public_file_server.enabled = false
config.cache_store = :null_store
config.action_cable.mount_path = nil
config.active_job.queue_adapter = :inline # async adapter はスレッドプールを持つので不可
```

→ 現時点では Action Cable やキャッシュストアなど、まだ使えない機能がある

## 3. Active Record: DB 接続は main Ractor に委譲

* `RactorConnectionHandler` / `ProxyConnectionPool` / 各 ProxyAdapter(9e8fe3955b、3900行超の大きな変更)
  * 非 main Ractor 用の ConnectionHandler。クエリの実行は main Ractor にディスパッチする
  * MySQL / PostgreSQL / SQLite3 それぞれの ProxyAdapter がある
  * テストスイート全体をこのプロキシ経由で流す `sqlite3_ractor` プロファイルも追加(c1fdd4e2cc)
* その他、モデルのクラスレベルの状態を ractor safe にする修正が多数
  * primary key、table_name、スキーマ情報(`SchemaContext` に集約: 51a2a815a2)
  * `find_by` の statement cache、`_default_attributes`、scope、default scope、enum、autosave、counter cache、readonly attributes
  * `PredicateBuilder`(main Ractor へのプロキシ → 後に ractor safe 化)、`Arel::Table`
  * reflection は Copy on Write にして deep freeze(a791f84224)

## 4. Action Pack / Action View

* コントローラ
  * `action_methods`、`controller_path`、`helper_method`、`add_flash_types`、`rescue_from`、middleware stack、`ParamsWrapper` などを ractor safe に
  * `ActionController::Renderer::RENDERERS` 定数を deprecate(41799bd0fd)。Renderer をミューテートしない形に変更
  * `ActionDispatch::ExceptionWrapper.wrapper_exceptions` / `silent_exceptions` を config 化して freeze できるように(CHANGELOG にも掲載)
* ルーティング / URL ヘルパーを Ractor 内から呼べるように(7f30cb3b51、0b1de97b43)
* Mime type registry、Response のデフォルトヘッダー、cookie store の SameSite デフォルトなどを shareable に
* Action View
  * テンプレートハンドラ、dependency tracker のレジストリを shareable に
  * テンプレート探索のキャッシュ(`LookupContext::DetailsKey` など)を Ractor ローカルストレージへ移動
  * FormHelper / AssetTagHelper などの設定値を、`mattr_accessor`(クラス変数)からシングルトンクラスの属性へ変更

## 5. Active Support のその他

* `class_attribute` を Ractor safe な実装に変更(9d4f4fa60c、John Hawthorn)。性能低下は10%以内
* `ActiveSupport::Callbacks` を ractor safe に(7edcc87b6a)
* Notifications(a6b2abf05e)、LogSubscriber、event reporter、backtrace cleaner、JSON encoder、inflection、TimeZone、TimeFormats、MessageVerifiers などを Ractor から使えるように
* Notifications の notifier 向けに `to_ractor_snapshot` / `load_ractor_snapshot` プロトコルを追加(bd9ea455eb、Ryuta Kamizono)

## 6. Active Model / Active Job

* Active Model: `model_name`、`to_partial_path`、属性の型、`alias_attribute`、`normalizes` などを Ractor 内から使えるように
* Active Job: queue adapter を shareable にし、ワーカー Ractor からジョブを perform できるように(d02de7704e、781fa22376)

## 7. テスト用ヘルパー

`ActiveSupport::Testing::RactorsAssertions`(nodoc)

* `on_ractor { ... }` ... ブロックを新しい Ractor で実行し、結果を返す(f68e6fbb22)
* `assert_ractor_shareable` / `assert_not_ractor_shareable` / `assert_ractor_make_shareable`

```ruby
ractorize!
assert_equal "Comment", on_ractor { Post.reflect_on_association(:comment).klass.name }
assert_equal "Hello, worker", on_ractor { HelloJob.perform_now("worker") }
```

## ユーザー目線でのまとめ(スライド化の候補)

* Rails 8.2 では Ractor 上でアプリを動かすための下地作りが大量に入っている(約160コミット)
* 公開 API はほぼ無く、ほとんどが nodoc の内部変更。既存アプリの挙動は基本的に変わらない
* 例外的にユーザーに見える変更
  * `ActionController::Renderer::RENDERERS` の deprecate
  * `config.action_dispatch.wrapper_exceptions` など、freeze するための config 追加
  * 各種設定値が boot 後に freeze されるようになったため、boot 後に書き換えているコードは FrozenError になりうる
* 本格的に使えるのはまだ先。Ruby 4.0以上、rack 側の対応、Action Cable / キャッシュストア / async adapter などは未対応
