## [Add `group` method to `ActiveSupport::ContinuousIntegration` for parallel step execution.](https://github.com/rails/rails/pull/56774)

* `ActiveSupport::ContinuousIntegration`にstepを並列実行する`group`メソッドを追加
* `group`には複数の`step`と並列実行数(`parallel:`)を指定する

```ruby
group "Checks", parallel: 3 do
  step "Style: Ruby", "bin/rubocop"
  step "Security: Brakeman", "bin/brakeman --quiet"
  step "Security: Gem audit", "bin/bundler-audit"
end
```

* `group`はネスト可能。並列実行されるのは外側の`group`のみで、内側の`group`はシーケンシャルに実行される。
