## [Add `has_json` and `has_delegated_json` to provide schema-enforced access to JSON attributes.](https://github.com/rails/rails/pull/56258)

* JSON属性にスキーマを強制して使用する為のAPI(`has_json`、`has_delegated_json`)を追加
* スキーマで使用出来る型は`boolean`、`integer`、`string`のみで、ネストなどは出来ない
* `has_delegated_json`は`has_json`と違い、スキーマのキーがそのままgetter/setterとして定義される
* スキーマは、型とデフォルト値どちらでも指定可能

---

## [Add `has_json` and `has_delegated_json` to provide schema-enforced access to JSON attributes.](https://github.com/rails/rails/pull/56258)

```ruby
create_table :accounts do |t|
  t.json :settings
  t.json :flags
  t.timestamps
end
```


```ruby
class Account < ApplicationRecord
  has_json :settings, restrict_creation_to_admins: true, max_invites: 10, greeting: "Hello!"
  has_delegated_json :flags, beta: false, staff: :boolean
end

a = Account.new
a.settings.restrict_creation_to_admins? # => true
a.settings.max_invites = 100
a.settings.max_invites = "text" # これはエラーにならず0で設定される

a.staff = true
a.staff? # => true
```
