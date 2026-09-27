## [Add `sql_notifications` connection configuration option to disable SQL notifications for specific database connections.](https://github.com/rails/rails/pull/56899)


* SQL notificationsをconnectionごとに無効化する`sql_notifications`オプションが追加された
  * `database.yml`の対象connectionに`sql_notifications: false`を指定すると無効化出来る
* これにより、Solid Cacheなどのライブラリが実行するSQLのログを無効化出来るようになっている
