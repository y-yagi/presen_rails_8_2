## [Add Rails.app.dotenvs to provide access to .env variables](https://github.com/rails/rails/pull/56455)

* `.env`ファイルから値を取得するための`ActiveSupport::DotEnvConfiguration`を追加
* `Rails.app.dotenvs`から参照できる
* 合わせて、development環境では`Rails.app.creds`も`.env`ファイルの値を参照するようになった
  * 値の参照順はENV、`.env`ファイル(developmentのみ)、credentialsの順
