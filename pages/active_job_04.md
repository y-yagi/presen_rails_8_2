## [Add `ActiveJob::DeserializationError::RecordNotFound`](https://github.com/rails/rails/pull/57742)

* 引数のデシリアライズ時、Global ID経由で参照しているレコードが見つからない場合に使用される例外(`ActiveJob::DeserializationError::RecordNotFound`)が追加された

---

## [Add `ActiveJob::DeserializationError::RecordNotFound`](https://github.com/rails/rails/pull/57742)

* 元々は`ActiveJob::DeserializationError`が使用されていたが、これは一時的なDB接続エラーなどの他の理由でも発生するため、`discard_on`に指定すると、本来破棄すべきでないエラーも破棄してしまう、という問題があった
* レコードが見つからない場合だけを明示的に破棄したい場合、今後は`ActiveJob::DeserializationError::RecordNotFound`を使うと良い


* ```ruby
  discard_on ActiveJob::DeserializationError::RecordNotFound
  ```
