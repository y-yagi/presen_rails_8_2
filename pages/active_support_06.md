## [`ActiveSupport::Cache::RedisCacheStore` entirely reimplemented.](https://github.com/rails/rails/pull/57004)

* `ActiveSupport::Cache::RedisCacheStore`の実装を`redis` gemから`redis-client` gemを使用する形にリファクタリング
* 接続に`redis`オプションを使用している場合は、古い実装(`DeprecatedRedisCacheStore`)が使われる
  * この古い実装はdeprecated扱いになっている
* オプションの指定方法が変更になっている(既存の指定方法はdeprecated)
