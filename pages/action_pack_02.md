## [Modern header-based CSRF protection.](https://github.com/rails/rails/pull/56350)

* CSRF protectionを`Sec-Fetch-Site` headerをチェックする方式で出来るよう変更
* デフォルトは`Sec-Fetch-Site` headerと従来のtoken両方をチェックする
    * 新規アプリではheaderのみ
* controllerでも挙動を指定出来る

---

## [Modern header-based CSRF protection.](https://github.com/rails/rails/pull/56350)

* [`Sec-Fetch-Site`](https://developer.mozilla.org/ja/docs/Web/HTTP/Reference/Headers/Sec-Fetch-Site)はFetch Metadata Request Headersの一つ
  * ブラウザがリクエストへ自動付与し、JSからは変更出来ない([forbidden header](https://fetch.spec.whatwg.org/#forbidden-request-header))
* この値を見るだけで、tokenを使わずにクロスサイトリクエストかどうか判定出来る

---

## [Modern header-based CSRF protection.](https://github.com/rails/rails/pull/56350)

```ruby
class ApplicationController < ActionController::Base
  # `Sec-Fetch-Site` headerだけをチェック
  protect_from_forgery using: :header_only, with: :exception

  # `Sec-Fetch-Site` headerが無い場合tokenでチェック
  protect_from_forgery using: :header_or_legacy_token, with: :exception
end
```

---

## [Modern header-based CSRF protection.](https://github.com/rails/rails/pull/56350)

* 設定される値は下記
  * `same-origin`: schemeとhostとportが全て一致
  * `same-site`: 登録可能ドメイン(site)が同じ(schemeやsubdomainは違ってもよい)
  * `cross-site`: 上記以外(別サイトからのリクエスト)
  * `none`: URLの直接入力やbookmarkなど、ユーザー操作によるアクセス

---

## [Modern header-based CSRF protection.](https://github.com/rails/rails/pull/56350)

* Railsは値ごとにリクエストの許可/拒否を判断するようになっている(`:header_only`時)
  * `same-origin` / `same-site`: 許可
  * `cross-site`: 拒否(`trusted_origins`に登録されたoriginのみ許可)
  * `none`: 拒否

---

## [Modern header-based CSRF protection.](https://github.com/rails/rails/pull/56350)

* GET/HEAD、および`QUERY`メソッドのリクエストはそもそも判定対象外(元々safeなメソッドのため)
* `:header_or_legacy_token`時は、`none`とheaderが無い場合のみ従来のtokenチェックにfallbackする
