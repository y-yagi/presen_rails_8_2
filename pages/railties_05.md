## [Add Rails.app.revision to provide a version identifier](https://github.com/rails/rails/pull/56490)

* version identifierを取得する`Rails.app.revision`が追加された
  * error reportingやmonitoring、cache keyなどに使う想定
* デフォルトでは`ENV["REVISION"]`、アプリのルートにある`REVISION`ファイル、gitのcommit hashの順で値を取得する
* `config.revision`で明示的に値を指定することも可能

```ruby
Rails.app.revision # => "3d31d593e6cf0f82fa9bd0338b635af2f30d627b"
```

```ruby
# config/application.rb
config.revision = ENV["GIT_SHA"]
```
