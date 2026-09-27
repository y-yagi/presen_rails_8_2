## [Marcel 2 for content type detection](https://github.com/rails/rails/pull/58549)

* content type検知に使用する`marcel`をv1からv2に更新
* より広く、より正確なMIMEタイプ検出になり、セキュリティも強化される
* エイリアスではなく正規のタイプが使われるようになる(例: `text/x-yaml` → `application/yaml`)
