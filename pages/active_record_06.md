## [Add query predicate expressions for Active Record types.](https://github.com/rails/rails/pull/58479)

* `ActiveRecord::Type::QueryPredicates`をincludeすることで、query predicateが値の比較に使うSQL式を型側で定義出来るようになった
* orderもこの式を通して行われる
* データベース上のストレージ表現がRubyのcastingやserializationとは別に、DB側の式で比較する必要がある場合に使用する
