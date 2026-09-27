## [Allow `config.active_storage.variant_processor` to be set to a transformer class.](https://github.com/rails/rails/pull/58384)

* `config.active_storage.variant_processor`に、`:vips`、`:mini_magick`、`:disabled`に加えて独自のクラスを指定できるようになった
* 指定するクラスは`ActiveStorage::Transformers::Transformer`が定義するインターフェースを実装する必要がある
* 組み込みのimage analyzerは`variant_processor`が`:vips`か`:mini_magick`の場合のみblobを受け付けるため、独自クラスを指定する場合は`config.active_storage.analyzers`にも対応するanalyzerを追加する必要がある
