## [Support dumping `schema_migrations` in `db/schema.rb`.](https://github.com/rails/rails/pull/58134)

* `db/schema.rb`に、実行済みの`schema_migrations`を出力出来るようになった
* `config.active_record.dump_schema_migrations`をtrueにすると、全バージョンが出力される
* ファイルに記載されたバージョンがDBに存在しない場合、load時にそのバージョンが作成される
* 出力順はデフォルトでバージョン文字列を反転させた値の昇順(`sort_by(&:reverse)`)
  * 末尾の桁でソートされるので、新しいmigrationが同じ位置に追加されにくく、マージコンフリクトを避けやすい
  * `config.active_record.dump_schema_migrations_sort_by`で変更可能

---

## [Support dumping `schema_migrations` in `db/schema.rb`.](https://github.com/rails/rails/pull/58134)

```ruby
ActiveRecord::Schema[8.2].define do
  # ...
end

ActiveRecord::Schema.load_schema_migrations(__FILE__)
__END__
20260716101900
20260716130752
20260716112003
```
