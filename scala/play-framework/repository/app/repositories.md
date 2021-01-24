# repositories

## ![@rokumura7](https://avatars3.githubusercontent.com/u/54521076?s=60&u=5c14b3915bbf56a507791b64774b66b3459512ee&v=4)app/repositories

Repositoryパターンは、データの永続層\(要はDBのことね\)の管理をビジネスロジックから切り離す方法で、非常によく使われるパターンです！  
で、そのRepositoryのインターフェース群をこのパッケージに格納していく想定です😏  
ControllerやServiceなどのRepositoryを利用する側としてはこのインターフェースだけ意識する、という形になるわけです👽

この実装を行うメリットとしては、

* 開発者がデータ永続層のことを気にせずビジネスロジックの改修を行える
* データ永続層を変えたいとか併用したいという要件がでてきても柔軟に対応できる というところかな。

例えば、  
MySQLからPostgreSQLにします！とか、一元管理していたDBを分割します！とか、なったとしてもRepositoryのみ修正すれば良いということになるわけですね👽  
要件変更が入ってもビジネスロジックだけ書き換えるのも用意だったり👽

ここらへんの記事が色々説明してくれてますね✨

{% embed url="https://qiita.com/mikesorae/items/ff8192fb9cf106262dbf" %}

{% embed url="https://docs.microsoft.com/ja-jp/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design" %}

{% embed url="https://docs.oracle.com/javase/jp/6/api/java/sql/Statement.html" %}



### repositories/Repository.scala

これはいわゆるマーカーインターフェースというもので単なる目印みたいなものですね😛  
クラスの分類を示せて、このクラスに属しているならこの処理、みたいな使い方はできますね☝️

[Wiki：マーカーインタフェース](https://ja.wikipedia.org/wiki/%E3%83%9E%E3%83%BC%E3%82%AB%E3%83%BC%E3%82%A4%E3%83%B3%E3%82%BF%E3%83%95%E3%82%A7%E3%83%BC%E3%82%B9)  


### repositories/impl

`app/repositories/impl`には、`repositories`で提供したインターフェースに対する具体的な実装を入れていく

```scala
trait Repository
trait HealthCheckRepository extends Repository
abstract class AbstractRepository extends Repository
class HealthCheckRepositoryImpl extends AbstractRepository with HealthCheckRepository
```

* `Respository`は単なるマーカー
* `HealthCheckRepository`は振る舞い定義
* `AbstractRepository`はRepositoryみんなが使う処理を抽象化。実装を提供。
* `HealthCheckRepositoryImpl`は`AbstractRepository`の力を借りて`HealthCheckRepository`の振る舞いを実装

{% hint style="warning" %}
`HealthCheckRepository`と`AbstractRepositoryは`親子関係にはない
{% endhint %}

### AbstractRepository

`abstract class`で定義されている。つまりこいつは実装を持っているわけだ。どんなRepositoryでも使うであろう「SQLを発行して結果を返す」という処理`execute`だね🙌

```scala
package repositories

import java.sql.ResultSet
import play.api.db.Database
import utils.Closable

abstract class AbstractRepository(protected val db: Database) extends Repository with Closable{
  def execute[A](sql: String)( func: ResultSet => A ): A = using(db.getConnection()) { con =>
    func(con.createStatement.executeQuery(sql))
  }
  def executeUpdate(sql: String)( func: Int => Int ): Int = using(db.getConnection()) { con =>
    func(con.createStatement.executeUpdate(sql))
  }
}
```

{% embed url="https://java-code.jp/969" %}

これまで書いていたexecuteとの違いは実際に実行しているメソッドが`executeQuery`か`executeUpdate`と言う点です。  
executeQueryは返り値にResultSetと言う型を返すのに対して、executeUpdateはIntを返します。

SELECT文の場合、キーバリューのようなレコードを取得する必要があり、これに使えるのがResultSetです。  
ResultSetの中にはSQLの実行結果が入っていて`resultSet.getString(カラム名)`で取得できた結果から各値を取り出せます。  
ちょうど連想配列で`hogeMap['id']`とするのと似た感覚です。

これに対して、登録・更新・削除の場合の実行結果とは何か。  
つまり、返り値となりうるものは、**処理に成功したレコード数**です。  
そのため先ほどのexecuteQueryは使えません。  
代わりに用意されているのがexecuteUpdate。  
データベースのテーブルからして見れば、登録・更新・削除、いずれにしてもテーブルの値がupdateされるわけなので。

{% embed url="https://java-code.jp/971" %}



### HealthCheckRepository

振る舞い定義。`HealthCheckRepositoryImpl`で実現をして、DIする

```scala
package repositories

import com.google.inject.ImplementedBy
import repositories.impl.HealthCheckRepositoryImpl

@ImplementedBy(classOf[HealthCheckRepositoryImpl])
trait HealthCheckRepository extends Repository {
  def canConnect: Boolean
}ca
```

`@ImplementedBy(classOf[HealthCheckRepositoryImpl])`はどのクラスにDIするかというペアリングのような設定記述になります！

`canConnect`は振る舞いの定義だけで何の振る舞いも持っていないよね👀  
`HealthCheck`っていう要件に対する振る舞いだけ、つまりリモコンの上っ面みたいなもんで、これがどう実装されているかは隠蔽されているわけです👽

### HealthCheckRepositoryImpl

`AbstractRepository`と`HealthCheckRepository`を実装\(継承\)しているから、  
`AbstractRepository`の処理は利用できるし、`HealthCheckRepository`の振る舞いを実現しなきゃいけないんだね。

今回のケースだと、HealthCheckをする上で`canConnect`という処理が必要なわけだけど、  
これはRepository全体が担保すべき仕事ではなくて、あくまでも「HealthCheckするために」必要な処理だから、  
`AbstractRepository`にインターフェースを用意することは要件的に違くて、`HealthCheckRepository`がそれは担うべきだよね🐷

次に増やすとすればImplだけが増えるはずかな！  
`HealthCheckRepository`がすべき`canConnect`という振る舞い自体は変わらなくて、それがどう実装されるかが切り替わるはずなので🙌

例えば、v1ではMySQLを使っていたけどv2ではPosgreSQLを使うことになったとしたら、

* HealthCheckRepositoryImpl（MySQL用）
* HealthCheckRepositoryV2Impl（PostgreSQL用）

って感じで実装されて、ControllerはHealthCheckRepository.canConnectって中のことは何も知らずに呼び出せるって感じかな！🐷  



