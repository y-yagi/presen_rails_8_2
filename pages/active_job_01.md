## [Jobs are now enqueued after transaction commit.](https://github.com/rails/rails/pull/55788)

* トランザクション内でジョブをenqueueした場合、コミット完了後にenqueueされるようになった
  * 元々ロールバックされたレコードに対してジョブが実行されてしまう問題があり、それを解消するための対応

---

## [Jobs are now enqueued after transaction commit.](https://github.com/rails/rails/pull/55788)

* Rails 8.2の新規アプリ、及び`config.load_defaults "8.2"`を指定したアプリでは、`config.active_job.enqueue_after_transaction_commit = true`がデフォルトになる
  * 既存アプリはアップグレード時に`config/initializers/new_framework_defaults_8_2.rb`内の設定のコメントを外すことでオプトインできる
