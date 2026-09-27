## [Add `delete: true` option to `Rails.cache.read` for atomic read-and-delete (only supported by Redis cache store).](https://github.com/rails/rails/pull/56807)

* `Rails.cache.read`に`delete: true`オプションを追加
  * 対応しているのはRedis cache storeのみ
* 指定した場合、Redisの[GETDEL](https://redis.io/docs/latest/commands/getdel/)コマンドを使用してreadとdeleteがatomicに行われる

```ruby
Rails.cache.write("otp", "123456")
Rails.cache.read("otp", delete: true)  # => "123456"
Rails.cache.read("otp")                # => nil
```
