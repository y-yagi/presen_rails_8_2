## [Fix tag parameter content being overwritten instead of combined with tag block content.](https://github.com/rails/rails/pull/56293)

* `tag.div("Hello ") { "World" }`のように、引数とブロックの両方でコンテンツを指定した場合の挙動を修正
    * 修正前はブロックのコンテンツで上書きされ、`<div>World</div>`になっていた
    * 修正後は両方の内容が結合され、`<div>Hello World</div>`になる

```ruby
tag.div("Hello ") { "World" }
# Before => <div>World</div>
# After  => <div>Hello World</div>
```
