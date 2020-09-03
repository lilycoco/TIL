# Controller

## Action

{% embed url="https://www.playframework.com/documentation/2.8.x/ScalaActions" %}

```scala
val echo = Action { request =>
  Ok("Got request [" + request + "]")
}
```

`Ok` は コンテントタイプ **text/plain** のレスポンスボディを含む、 ステータス **200 OK** のレスポンスを生成する

## Dependency Injection

{% embed url="https://qiita.com/pab\_tech/items/1c0bdbc8a61949891f1f" %}

## CSRF

{% embed url="https://www.playframework.com/documentation/2.8.x/ScalaCsrf" %}

攻撃者が被害者のブラウザに被害者のセッションを使ったリクエストを行わせるように仕向ける、セキュリティ上の脆弱性のこと



