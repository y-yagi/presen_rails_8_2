## [Introduce `config.active_storage.draw_direct_upload_route` to disable the direct upload route without affecting the other Active Storage routes.](https://github.com/rails/rails/pull/58377)

* direct uploadのrouteだけを無効化できる設定が追加された
* 認証やバリデーションを経由しないアップロードを許可したくない時用
* 無効にすると、Action Textの`rich_textarea`は明示的に指定しない限り`data-direct-upload-url`を出力しなくなる
  * この属性が無いTrixエディタは添付ボタンを隠し、ドロップやペーストによるファイル添付も無視するようになっている
