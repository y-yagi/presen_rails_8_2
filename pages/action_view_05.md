## [Add a `herb:check` rake task to verify that the application's HTML+ERB templates compile through Herb.](https://github.com/rails/rails/pull/58770)

* `bin/rails herb:check`で、アプリケーション内の全てのHTML+ERBテンプレートがHerbでコンパイル出来るかを確認出来る
  * Herbを有効にする前に、既存のテンプレートに問題がないかを確認出来る
* Herbがコンパイルを拒否したテンプレートは、パスとエラー内容が一覧で出力される
