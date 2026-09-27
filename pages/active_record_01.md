## [Include record ID in error when uniqueness validation fails](https://github.com/rails/rails/pull/55826)

* uniqueness validationでエラーになった場合、コンフリクトしたrecordのidをエラーに含むようになった

```ruby
# Before
errors.details[:name]
# => [{error: :taken, value: "John Doe"}]

# After
errors.details[:name]
# => [{error: :taken, value: "John Doe", existing_id: 123}]
```
