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

{% embed url="http://lab.astamuse.co.jp/entry/2018/03/21/114500" %}

{% embed url="https://qiita.com/agoetc/items/c6068443d98de5180efd" %}



攻撃者が被害者のブラウザに被害者のセッションを使ったリクエストを行わせるように仕向ける、セキュリティ上の脆弱性のこと

PlayFrameworkの初期設定ではCSRF対策が必須で、view内でフォームを使用する場合、トークンを明示的に渡さなくてはいけません。  
また、CSRFを使用するときはRequestHeaderが必要

```scala
@import helper._
@()(implicit requestHeader: RequestHeader)

<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<script src="@routes.Assets.versioned("javascripts/main.js")" type="text/javascript"></script>

@form(CSRF(routes.HomeController.post())) {
    <div>
        <label for="name">なまえ</label>
        <input type="text" id="name" name="name" placeholder="なまえ">
    </div>
    <div class="button">
        <button type="button">送信</button>
    </div>
}
```

## Controller

元々は`Controller`と言うクラスしかなかったが、2.6で`Controller`がdeprecateして、`BaseController`・`AbstractController`・`InjectedController`の3つに生まれ変わった。

**BaseController:**  `AbstractController`と`InjectedController`の親で、カスタマイズ性の高いController。そのカスタマイズ性故に、`BaseController`には宣言だけされた`ControllerComponents`が存在して、利用する場合にはこの`ControllerComponents`の実装が強制される。

**AbstractController:** `ControllerComponents`をコンストラクタ引数として受け取れるためDIすることで`ControllerComponents`を気にすることなく実装ができる。 とは言えどちらにせよDIによって`ControllerComponent`の件は解消できる。

**InjectedController:** `ControllerComponent`を`setControllerComponents`で設定されるControllerで密結合になるから利用に注意したほうが良い。テストがしづらいと言うこと。 

**結論、特殊な事情がない限りAbstractControllerを使うのがベター**

