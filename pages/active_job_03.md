## [Add `ActiveJob::Attributes` for declaring typed attributes that persist across job serialization and deserialization](https://github.com/rails/rails/pull/57208)

* ジョブにActive Modelの`attribute`と同様の書き方で型付きの属性を定義できるようになった
* 定義した属性はジョブのシリアライズ、デシリアライズ時に自動で保持される
* `ActiveJob::Continuable`に組み込まれており、ジョブが中断、再開された場合も属性の値が保持される

---

## [Add `ActiveJob::Attributes` for declaring typed attributes that persist across job serialization and deserialization](https://github.com/rails/rails/pull/57208)

```ruby
class SubmitEnrollmentJob < ApplicationJob
  include ActiveJob::Continuable

  attribute :payment_token, :string
  attribute :billing_profile_id, :integer

  def perform(enrollment)
    step(:tokenize_payment_instrument) do
      self.payment_token = PaymentGateway.tokenize(enrollment.user.payment_instrument)
      # self.payment_tokenは以降のstepで使用可能
    end
  end
end
```
