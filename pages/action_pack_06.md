
## [Add support for the HTTP QUERY method defined in RFC 10008.](https://github.com/rails/rails/pull/57973)

* [RFC 10008](https://www.rfc-editor.org/rfc/rfc10008)で追加されたHTTP QUERYメソッドをサポート
* 他のHTTP methodと同様にroutingで指定出来る

```ruby
# config/routes.rb
query "search", to: "search#index"
match "filter", to: "search#filter", via: :query

request.query?                # => true
request.request_method_symbol # => :query
```

* database selector middlewareなどでも、GETやHEADと同様にreplicaから取得する対象として扱われる
  * その他HTTPメソッドにより挙動が変わる箇所は一通り対応されている

---

## [Add support for the HTTP QUERY method defined in RFC 10008.](https://github.com/rails/rails/pull/57973)

* GETのようにsafeかつ冪等だが、POSTのようにrequest bodyにクエリを載せられるHTTP method
* GETはURLの長さ制限があり長大なクエリを表現しづらい、POSTはsafe/冪等であることを表現出来ない、という問題を解決するために新設された
* レスポンスはキャッシュ可能

```
QUERY /search HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

q=foo&limit=10&sort=-published
```
