## [Add `config.action_dispatch.strict_accept_header` to stop forcing an HTML response when the `Accept` header contains the `*/*` wildcard.](https://github.com/rails/rails/pull/57579)

* `Accept`ヘッダに`*/*`ワイルドカードが含まれていても具体的なformatを使用するか制御する`config.action_dispatch.strict_accept_header`を追加
* IE7対応の名残で、`*/*`を含む`Accept`ヘッダはブラウザからのものとみなし常にHTMLを返していた
* 有効にした場合、`Accept: application/json, */*`のようなリクエストにJSONを返すようになる
