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

{% embed url="https://qiita.com/hshimo/items/1136087e1c6e5c5b0d9f" %}



