## [Support proc and symbol for `NumericalityValidator`s `:in` option](https://github.com/rails/rails/pull/46262)

* `NumericalityValidator`の`:in`オプションに、procやsymbolを指定出来るようになった

```ruby
validates_numericality_of :price, in: ->(o) { 0..o.max_price }
```

```ruby
validates_numericality_of :price, in: :price_range

def price_range
  0..max_price
end
```
