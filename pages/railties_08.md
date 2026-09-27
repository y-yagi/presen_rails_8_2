## [Enable Ruby frozen_string_literal by default](https://github.com/rails/rails/pull/57252)

* 新規アプリ作成時に、frozen string literalsを有効にする設定を行う`config/bootsnap.rb`を追加

```ruby
# config/bootsnap.rb

# Enable frozen string literal across the app, but not dependencies.
# This configuration should be kept in sync with
# `AllCops/StringLiteralsFrozenByDefault` in `.rubocop.yml`
Bootsnap.enable_frozen_string_literal(app_only: true)
```
---

## [Enable Ruby frozen_string_literal by default](https://github.com/rails/rails/pull/57252)

* `frozen_string_literal`の指定が無い場合、`frozen_string_literal: true`になる
  * 明示的に`false`が指定されている場合、そちらが優先される
* この設定はアプリコードのみに影響し、gemには影響しない
