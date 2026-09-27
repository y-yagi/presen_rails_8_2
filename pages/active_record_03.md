## [Add `#default_order` query method and association option which can be used to order records when no other order is specified.](https://github.com/rails/rails/pull/39525)

* 他のorderが指定されていない場合のdefault orderを、`#default_order`メソッドで指定出来るようになった
* associationのオプションとしても同じ機能を指定出来る

```ruby
class Post < ApplicationRecord

  has_many :comments, default_order: :likes
  # or
  has_many :comments, -> { default_order(:likes) }
end

Post.default_order("email DESC")

post = Post.first
post.comments # likesでorderされる
```
