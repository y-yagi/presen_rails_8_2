## [Introduce `ActiveStorage::Attachment` upload callbacks](https://github.com/rails/rails/pull/56327)

* blobのアップロード完了後に発火する`after_upload`コールバックが追加された
* `process: :immediately`のvariant生成、及びblobのanalysisは、アップロード済みのローカルファイルをそのまま使うようになり、再ダウンロードは不要

```ruby
ActiveStorage::Attachment.after_upload do
  # Your custom logic here
end
```
