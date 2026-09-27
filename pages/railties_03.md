## [Add Rails.app.creds to provide combined credentials lookup](https://github.com/rails/rails/pull/56404)

* ENVとencrypted credentialsの両方から値を取得する`ActiveSupport::CombinedConfiguration`を追加
* `Rails.app.creds`から参照でき、まずENVをチェックし、無ければencrypted credentialsから取得する

```ruby
Rails.app.creds.require(:db_host)
# ENV.fetch("DB_HOST") || Rails.app.credentials.require(:db_host)
```
