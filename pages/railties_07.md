## [Add offline fallback page to the PWA scaffold](https://github.com/rails/rails/pull/57184)

* 新規アプリに`app/views/pwa/offline.html.erb`テンプレートを追加
* `config/routes.rb`にコメントアウトされた`get "offline"`ルートを追加
* `service-worker.js`にもオフラインページをキャッシュ・配信するためのコメントアウトされた例が追加されている
