## [Define `ActionController::Parameters#deconstruct_keys` to support pattern matching](https://github.com/rails/rails/pull/55789)

* `ActionController::Parameters`でpattern matchingが出来るようになった

```ruby
if params in { search:, page: }
  Article.search(search).limit(page)
else
  # ...
end

case (value = params[:string_or_hash_with_nested_key])
in String
  # Stringの`value`に対する処理
in { nested_key: }
  # nested_keyを持つHashに対する処理
end
```
