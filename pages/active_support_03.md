## [Add `prepend: true` option to `ActiveSupport::Notifications.subscribe`.](https://github.com/rails/rails/pull/57115)

* `ActiveSupport::Notifications.subscribe`に`prepend: true`オプションを追加
* 指定したsubscriberをイベントのsubscriberリストの先頭に追加する
* 先頭に追加されたsubscriberは他のsubscriberより先に実行される
  * 特定のイベントに共通の値を設定するなどがしやすくなっている

```ruby
ActiveSupport::Notifications.subscribe("sql.active_record", prepend: true) do |event|
  event.payload[:name] = "[IDC] #{event.payload[:name]}"
end
```
