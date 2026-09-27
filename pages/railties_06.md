## [Add bin/rails query command for running read-only database queries](https://github.com/rails/rails/pull/57156)

* read-only databaseに対してqueryを実行する`bin/rails query`コマンドを追加
* Active Recordの式、及びSQLを実行でき、結果はJSONで返る
  * エラーもJSON
* readingのreplicaに接続し、書き込みは行えない
* schemaやEXPLAINを実行するサブコマンドも提供

---

## [Add bin/rails query command for running read-only database queries](https://github.com/rails/rails/pull/57156)

```bash
$ bin/rails query "Account.limit(10)"
# => {"columns":["id","settings","flags","created_at","updated_at"],"rows":[],"meta":{"row_count":0,"query_time_ms":18.5,"page":1,"per_page":100,"has_more":false,"sql":"SELECT \"accounts\".* FROM \"accounts\" LIMIT 10"}}

$ bin/rails query "Account.typo(10)"
# => {"error":"undefined method 'typo' for an instance of ActiveRecord::Relation","meta":{"query_time_ms":0}}
```

```bash
$ bin/rails query --sql "SELECT COUNT(*) FROM accounts"
$ bin/rails query schema accounts
$ bin/rails query explain "Account.where(plan: 'premium')"
```
