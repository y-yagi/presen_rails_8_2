## [Allow query log tags to be configured per connection pool.](https://github.com/rails/rails/pull/57641)

* query log tagのformatを、connection poolごとに指定出来るようになった
* `prepend_comment`もconnection poolごとに指定可能

```yaml
production:
  primary:
    database: primary
  analytics:
    database: analytics
    query_log_tags:
      format: sqlcommenter
```
