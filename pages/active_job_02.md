## [Allow `retry_on` `wait` procs to accept the error as a second argument.](https://github.com/rails/rails/pull/56601)

* `retry_on`の`wait`オプションに指定するprocで、発生したエラーを第2引数として受け取れるようになった

```ruby
class RemoteServiceJob < ActiveJob::Base
  retry_on CustomError, wait: ->(executions, error) { error.retry_after || executions * 2 }

  def perform
    # ...
  end
end
```
