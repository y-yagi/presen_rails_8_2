## [Add `config.active_record.shuffle_unordered_selects`.](https://github.com/rails/rails/pull/58548)

* `ORDER BY`を指定していない`SELECT`は本来行の順序が保証されないが、実際にはRDBMSが一貫した順序で返す事が多く、コードやテストがその順序に依存してしまう事があった
  * そしてRDBMSのバージョンアップなどで苦労する
* `config.active_record.shuffle_unordered_selects`を有効にすると、`ORDER BY`のないクエリの結果を可能な範囲でshuffleするようになる

---

## [Add `config.active_record.shuffle_unordered_selects`.](https://github.com/rails/rails/pull/58548)

```ruby
config.active_record.shuffle_unordered_selects = true
```

* これにより、`ORDER BY`が指定されていないクエリの検出がしやすくなっている
  * テスト環境での使用を想定
