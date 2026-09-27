## [Introduce `ActiveSupport::TimeFormats` and `ActiveSupport::DateFormats` for registering custom date formats.](https://github.com/rails/rails/pull/57345)

* custom date formatを登録する為の`ActiveSupport::TimeFormats`、`ActiveSupport::DateFormats`を追加
  * 元々は`Time::DATE_FORMATS`、`Date::DATE_FORMATS`定数に直接値を追加する必要があった
* 今後はこれらのAPIを使用して登録する必要がある(定数を直接編集するのはdeprecated)

---

## [Introduce `ActiveSupport::TimeFormats` and `ActiveSupport::DateFormats` for registering custom date formats.](https://github.com/rails/rails/pull/57345)

```ruby
ActiveSupport::TimeFormats.register(:month_and_year, '%B %Y')
ActiveSupport::DateFormats.register(
  :short_ordinal,
  ->(date) { date.strftime("%B #{date.day.ordinalize}") }
)
```

* 後述するRactorサポートの影響で、定数はfreezeされ直接変更するという事は出来なくする方針
* そのために、同様に定数を編集する機能はdeprecate+変更用のAPIを提供、という変更が他の機能でも行われている
