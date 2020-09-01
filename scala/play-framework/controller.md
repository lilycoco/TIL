# Controller

{% embed url="https://www.playframework.com/documentation/2.8.x/ScalaActions" %}

## Action

```scala
val echo = Action { request =>
  Ok("Got request [" + request + "]")
}
```

`Ok` は コンテントタイプ **text/plain** のレスポンスボディを含む、 ステータス **200 OK** のレスポンスを生成する



