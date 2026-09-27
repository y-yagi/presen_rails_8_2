## [Allow setting `config.action_view.erb_implementation` to `:herb` to compile HTML+ERB templates through Herb.](https://github.com/rails/rails/pull/58721)

* `config.load_defaults 8.2`を指定した場合、HTML formatのテンプレートは[Herb](https://herb-tools.dev/)でコンパイルされるようになる
    * 閉じタグ忘れなど構造上の問題があるテンプレートは、コンパイル時にテンプレートの場所付きでエラーになる
    * 問題がないテンプレートは、Erubiを使った場合と同じ出力になる
---

## [Herb](https://herb-tools.dev/)

* [Marco Roth](https://github.com/marcoroth)氏が開発している、HTML+ERB向けのツール群
* Erubiはテンプレートを単なるテキストとして扱うが、HerbはHTMLの構造(タグの対応関係など)も含めて解析する
    * その為、閉じタグ忘れなどの問題をコンパイル時に検出出来る
* パーサーの上に、ERBのコンパイラ(`Herb::Engine`)やLinter、Formatter、Language Serverなどが提供されている
* [動画](https://kaigionrails.org/2025/talks/marcoroth/#day2)だとわかりやすい

---

## [Allow setting `config.action_view.erb_implementation` to `:herb` to compile HTML+ERB templates through Herb.](https://github.com/rails/rails/pull/58721)

* HTML format以外のテンプレートは、引き続きErubiでコンパイルされる
* `config.action_view.erb_implementation`を`:erubi`に設定すれば、全てのテンプレートをErubiでコンパイルする従来の挙動に戻せる
