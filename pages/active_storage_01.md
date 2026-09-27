## [Introduce immediate variants that are generated immediately on attachment](https://github.com/rails/rails/pull/51951)

* variantの生成タイミングを指定する`process`オプションが追加された
  * `:lazily`(デフォルト): リクエスト時に生成
  * `:later` アタッチ後、バックグラウンドジョブで生成
  * `:immediately` アタッチと同時に生成

---

## [Introduce immediate variants that are generated immediately on attachment](https://github.com/rails/rails/pull/51951)

* 元々あった`preprocessed: true`は非推奨になり、今後は`process: :later`を使う必要がある

```ruby
has_one_attached :avatar do |attachable|
  attachable.variant :thumb, resize_to_limit: [100, 100], process: :immediately
end
```
