## [Add a configuration for `ActionDispatch::ExceptionWrapper.wrapper_exceptions` at `config.action_dispatch.wrapper_exceptions`.](https://github.com/rails/rails/pull/57483)

* `ActionDispatch::ExceptionWrapper.wrapper_exceptions`に値を設定する為のconfig、`config.action_dispatch.wrapper_exceptions`を追加
* 同様に`ActionDispatch::ExceptionWrapper.silent_exceptions`用の`config.action_dispatch.silent_exceptions`も追加
* これまでは対象の定数に直接値を追加する必要があったが、configで設定出来るようになった
