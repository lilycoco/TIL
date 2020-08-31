# Model

{% embed url="https://www.playframework.com/documentation/2.7.x/ScalaDatabase" %}

## 　Library dependencies

{% embed url="https://www.playframework.com/documentation/ja/2.4.x/SBTDependencies" %}

`build.sbt` 

```text
libraryDependencies ++= Seq(
  "org.apache.derby" % "derby" % "10.11.1.1",
  "org.hibernate" % "hibernate-entitymanager" % "4.3.9.Final"
)

libraryDependencies += "org.apache.derby" % "derby" % "10.11.1.1" % "test"
```

\(`group`, `artifact`, `revision` , `configuration`\) を定義している

## Database

### evolutions

{% embed url="https://www.playframework.com/documentation/2.8.x/Evolutions\#Managing-database-evolutions" %}

DBマイグレーションツール

スキーマのバージョン管理のようなもの。スキーマの変更に対してデータベースを追随させることができる。evolutionsはPlayframeworkで使われているDBマイグレーションツール

## ORM

{% hint style="info" %}
ORMとは

1. データベースからデータを取得する
2. 取得したデータをオブジェクト化する
3. データの更新・変更などをデータベースに格納する

[オブジェクト関係マッピング](https://qiita.com/yk-nakamura/items/acd071f16cda844579b9)
{% endhint %}

### Anorm

{% embed url="https://playframework.github.io/anorm/" %}

SQLの実行/結果解析をサポートする、データベースアクセスのためのライブラリ

### Skinny-ORM \(ORM: Object-relational mapping\)

{% embed url="http://skinny-framework.org/documentation/orm.html" %}

Skinny-ORMによってJOINが簡単になる

