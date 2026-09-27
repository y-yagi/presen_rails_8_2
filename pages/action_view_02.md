## [Add `datalist_tag` to create `datalist` form elements.](https://github.com/rails/rails/pull/52137)

* `datalist` elementを作成する為の`datalist_tag`メソッドを追加

```ruby
datalist_tag('countries_datalist', ['Argentina', ['Brazil', { class: 'brazilian_option' }], ['Chile', 'CL', { disabled: true }]], { class: 'sa-countries-sample' })
# => <datalist id="countries_datalist" class="sa-countries-sample">
#      <option value="Argentina">Argentina</option>
#      <option value="Brazil" class="brazilian_option">Brazil</option>
#      <option value="CL" disabled="disabled">Chile</option>
#    </datalist>
```
