## [Add built-in Argon2 support for `has_secure_password`.](https://github.com/rails/rails/pull/56057)

* `has_secure_password`のアルゴリズムとして、bcryptに加えてArgon2が組み込みで使えるようになった
* Gemfileに`gem "argon2", "~> 2.3"`を追加し、`algorithm: :argon2`を指定するだけで使える
* 合わせて、独自のアルゴリズムを登録する為の`ActiveModel::SecurePassword.register_algorithm` APIも追加された

```ruby
class User < ActiveRecord::Base
  has_secure_password algorithm: :argon2
end
```

---

## Argon2

* パスワードハッシュ化の為のアルゴリズム
    * RFC 9106で標準化された
* 計算時間だけでなく、使用するメモリ量や並列度もパラメータで指定出来る
    * 大量のメモリを必要とするようにすることで、GPUや専用ハードウェアによる総当たり攻撃のコストを上げられる
* `has_secure_password`がデフォルトで使っているbcryptには、入力長が72バイトまでという制限があるが、Argon2にはない

---

## [Add built-in Argon2 support for `has_secure_password`.](https://github.com/rails/rails/pull/56057)

* Argon2d、Argon2i、Argon2idの3つの種類があり、OWASPはパスワードの保存にArgon2idを推奨している
* 今回の対応では、[argon2](https://github.com/technion/ruby-argon2) gemを使うようになっており、このgemはデフォルトでArgon2idを使用するようになっている
