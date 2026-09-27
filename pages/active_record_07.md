## [Add `config.active_record.schema_ignored_tables` to exclude tables from both the schema cache and the schema file.](https://github.com/rails/rails/pull/58554)

* 従来、テーブルをschema cacheとschema fileの両方から除外するには、`schema_cache_ignored_tables`と`SchemaDumper.ignore_tables`の2つの設定が必要だった
* `config.active_record.schema_ignored_tables`で、両方まとめて除外出来るようになった

---

## [Add `config.active_record.schema_ignored_tables` to exclude tables from both the schema cache and the schema file.](https://github.com/rails/rails/pull/58554)

* テーブル名は`table_name_prefix`/`table_name_suffix`を含む実際のDB上の名前でマッチする
    * `SchemaDumper.ignore_tables`は従来prefix/suffixを除いた名前でマッチしていたので、微妙に挙動が違う
