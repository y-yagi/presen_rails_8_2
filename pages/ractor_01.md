## Ractorとは

* Ruby 3.0で導入された、並列処理の為の仕組み
    * Actorモデルをベースにしている
* Threadは GVL(Global VM Lock)の影響で Rubyコードを同時に1つしか実行出来ないが、Ractorは Ractorごとにロックを持つので、複数の Ractorで Rubyコードを並列に実行出来る
* 並列実行を安全に行う為に、Ractor間ではオブジェクトの共有が制限されている
    * Ractor間のやりとりは基本的にメッセージの送受信で行う(Ruby 4.0から`Ractor::Port`という機能が提供されている)

---

## Ractorサポート

* Rails アプリケーションを Ractor上で動かすための対応が、現在進行中で進んでいる
  * Shopifyが頑張っている
* Ruby 4.0以上前提
  * 4.0で追加されたAPI(`Ractor.shareable_proc` / `Ractor::Port`)を使用している

---

## Ractorサポート

* Ractor 間で共有できるのは shareable なオブジェクト(deep freeze されたもの、クラス/モジュールなど)だけ
* Rails はクラスレベルの状態(設定値、キャッシュ、コールバック用の Procなど)を大量に持っているので、頑張ってfreezeしたりRactorローカルストレージを使うよう変更したりしている
  * 先に触れた定数のfreeze化対応もこの影響
* どうしても main Ractor でしか扱えない処理(DB 接続など)は main Ractorに処理を委譲して実行
  * 委譲処理は、[jhawthorn/ractor-dispatch](https://github.com/jhawthorn/ractor-dispatch)を使用

---

## Ractorサポート

* この資料作成時点では、experimentalでprivate API扱い
* Rails 8.2でpublic APIになるかはわからないが、ライブラリなどでRactorサポートをしたいがどうすればわからない、というときの参考になる(かもしれない)
