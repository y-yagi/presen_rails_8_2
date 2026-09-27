## [Allow HTTP token authentication to require specific authentication schemes.](https://github.com/rails/rails/pull/58211)

* token authenticationで、認証を許可するスキームを`scheme`オプションで指定出来るよう変更
* 指定した値と異なるスキームだった場合、認証は行われない(認証失敗になる)

```ruby
authenticate_or_request_with_http_token(scheme: ["Bearer", "DPoP"]) do |token, options, scheme|
  # ...
end
```
