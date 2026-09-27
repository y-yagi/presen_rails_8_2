## [Offload ActiveStorage::Blob#metadata sync to background](https://github.com/rails/rails/pull/57060)

* httpサーバーとバックグラウンドワーカー(`ActiveStorage::AnalyzeJob`)が同時にblobのmetadataを更新しようとすると、GCSで409エラーが発生することがあった
* metadataの更新処理を新しい`ActiveStorage::SyncMetadataJob`にオフロードすることで、リクエスト側が409で500エラーになるのを防ぐ

---

## [Offload ActiveStorage::Blob#metadata sync to background](https://github.com/rails/rails/pull/57060)

* カスタムサービスを使っている場合、コンフリクトを再試行したいときは`ActiveStorage::SyncMetadataJob`に`retry_on`を追加できる
* `Service#update_metadata`は処理を`update_metadata_for`に委譲するようになった
  * `update_metadata`を上書きしていたカスタムサービスは`update_metadata_for`を上書きするよう変更が必要
