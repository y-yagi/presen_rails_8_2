## [Add `to_markdown` to Action Text, mirroring `to_plain_text`.](https://github.com/rails/rails/pull/56858)

* リッチテキストのコンテンツをMarkdownに変換する`to_markdown`が追加された
* 見出し、太字、斜体、取り消し線、インラインコード、コードブロック、引用、リスト、リンク、テーブル、添付ファイルに対応

```ruby
message = Message.create!(content: "<h1>Hello</h1><p>This is <strong>bold</strong></p>")
message.content.to_markdown # => "# Hello\n\nThis is **bold**"
```

---

## [Add `to_markdown` to Action Text, mirroring `to_plain_text`.](https://github.com/rails/rails/pull/56858)

* 添付ファイルの変換方法は、対象モデルに`attachable_markdown_representation`を実装することでカスタマイズできる
* `<action-text-markdown>`要素はAction Text内部で予約されており、コンテンツからは除去される
