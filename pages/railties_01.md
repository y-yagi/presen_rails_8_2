## [Add Rails.app as alias for Rails.application](https://github.com/rails/rails/pull/56403)

* `Rails.application`のaliasとして`Rails.app`を追加
* `Rails.application.credentials`のように、ネストしたaccessorを使う場合に短く書けて便利

```ruby
Rails.app.credentials
```
