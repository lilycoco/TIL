# Repository

{% embed url="https://www.playframework.com/documentation/2.8.x/Anatomy\#The-Play-application-layout" %}



```markup
app                      → Application sources
 └ assets                → Compiled asset sources
    └ stylesheets        → Typically LESS CSS sources
    └ javascripts        → Typically CoffeeScript sources
 └ controllers           → Application controllers
 └ models                → Application business layer
 └ views                 → Templates
 └ repositories(Optional)→ 
    └ impl               → 
build.sbt                → Application build script
conf                     → Configurations files and other non-compiled resources (on classpath)
 └ application.conf      → Main configuration file
 └ routes                → Routes definition
dist                     → Arbitrary files to be included in your projects distribution
public                   → Public assets
 └ stylesheets           → CSS files
 └ javascripts           → Javascript files
 └ images                → Image files
project                  → sbt configuration files
 └ build.properties      → Marker for sbt project
 └ plugins.sbt           → sbt plugins including the declaration for Play itself
lib                      → Unmanaged libraries dependencies
logs                     → Logs folder
 └ application.log       → Default log file
target                   → Generated stuff
 └ resolution-cache      → Info about dependencies
 └ scala-2.13
    └ api                → Generated API docs
    └ classes            → Compiled class files
    └ routes             → Sources generated from routes
    └ twirl              → Sources generated from templates
 └ universal             → Application packaging
 └ web                   → Compiled web assets
test                     → source folder for unit or functional tests
```

## ![@rokumura7](https://avatars3.githubusercontent.com/u/54521076?s=60&u=5c14b3915bbf56a507791b64774b66b3459512ee&v=4)Repositories

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

  
  


## repositories/impl

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

###  AbstractRepository

`abstract class`で定義されている。つまりこいつは実装を持っているわけだ。どんなRepositoryでも使うであろう「SQLを発行して結果を返す」という処理`execute`だね🙌

###  HealthCheckRepository

振る舞い定義。`HealthCheckRepositoryImpl`で実現をして、DIする

`trait`で振る舞いの定義だけで何の振る舞いも持っていないよね👀  
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



