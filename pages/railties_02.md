## [Add Rails.app.envs to provide access to ENV variables](https://github.com/rails/rails/pull/56404)

* ENV変数をsymbolベースで取得するための`ActiveSupport::EnvConfiguration`を追加
* `Rails.app.envs`から参照できる
* 値が必須かどうかを`require`/`option`で明示的に指定できる

```ruby
Rails.app.envs.require(:db_password)  #=> key not found: "DB_PASSWORD" (KeyError)
Rails.app.envs.option(:db_password)  #=> nil
```

```ruby
Rails.app.envs.require(:aws, :access_key_id) # ENV.fetch("AWS__ACCESS_KEY_ID")　と同じ
Rails.app.envs.option(:database, :host, default: -> { "missing" }) # => ENV.fetch("DATABASE__HOST") { "missing" } と同じ
```
