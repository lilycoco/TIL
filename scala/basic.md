# Basic

```scala
class Calc {
  val sum = (x: Int, y: Int) => x + y
}
​
val c = new Calc
println(c.sum(1, 2))
​
// 関数
// オブジェクト
val sum = (x: Int, y: Int) => x + y
val double = (x: Int) => x * 2
val half = (x: Int) => x / 2
​
// メソッド
// オブジェクトの所有物 = オブジェクトの振る舞い
// 人が車使う、挨拶する
def sum (x: Int, y: Int): Int = x + y
def methodHalf (x: Int): Int = x / 2
​
val addThenX = (x: Int, y: Int) => (func: Int => Int) => func(x + y)
println(addThenX(1, 2) { r =>
  r * 2
})
​
val addThenY = (x: Int, y: Int, func: Int => Int) => func(x + y)
println(addThenY(1, 2, { r =>
  r * 2
}))
​
// class
class Dog(name: String)
val dog1 = new Dog("poti")
val dog2 = new Dog("poti")
// インスタンスを比較
if (dog1 == dog2) println("同じ犬")
else println("違う犬") // 違う犬
​
// case class
case class CaseDog(name: String)
val cDog1 = CaseDog("poti")
val cDog2 = CaseDog("poti")
// 値を比較
if (cDog1 == cDog2) println("同じ犬")
else println("違う犬") // 同じ犬
​
case class Coin(num: Int)
​
case class Name(value: String)
class User(name: Name) {
  def getName: Name = name
}
​
val user1 = new User(Name("遼子"))
val user2 = new User(Name("遼子"))
//val user3 = new User("遼子")
​
// インスタンスを比較
if (user1 == user2) println("同じユーザー")
else println("違うユーザー")
// 値を比較
if (user1.getName == user2.getName) println("同じ名前")
else println("違う名前")
​
// object
// 単一のインスタンスを持つクラス（シングルトン）
object Counter {
  private var num = 0
  def add: Unit = num += 1
  def getNum(): Int = num
}
​
Counter.add
println(Counter.getNum())
Counter.add
println(Counter.getNum())
Counter.add
Counter.add
println(Counter.getNum())
​
class ClassCounter {
  private var num = 0
  def add: Unit = num += 1
  def getNum(): Int = num
}
val counter1 = new ClassCounter
counter1.add
println(counter1.getNum())
counter1.add
println(counter1.getNum())
​
val counter2 = new ClassCounter
println(counter2.getNum())
​
// trait
// 実装とフィールドを持てる型
// 振る舞いだけを定義して、子クラスに実装を強制する
trait Engineer {
  def greet(): Unit = println("こんにちは")
  def develop(): Unit
}
class FrontendEngineer extends Engineer {
  override def develop(): Unit = println("フロントエンドの開発")
}
class BackendEngineer extends Engineer {
  override def greet(): Unit = println("はろー")
  override def develop(): Unit = println("バックエンドの開発")
}
​
val members: Seq[Engineer] = Seq(
  new FrontendEngineer,
  new FrontendEngineer,
  new BackendEngineer,
)
​
members.foreach(member => member.develop())
members.foreach(member => member.greet())
​
// abstract
// traitと同様に抽象メソッド（＝実装がされていないメソッド）を持てる
// コンストラクタを持てる
abstract class Developer(name: String) {
  def greet(): Unit = println("こんにちは")
  def develop(): Unit
}
​
trait Engineer {
  def develop(): Unit
}
​
abstract class Person(name: String) {
  def greet(): Unit = println(s"こんにちは、${name}です")
}
​
class FrontendEngineer(name: String) extends Person(name) with Engineer {
  override def develop(): Unit = println("フロントエンドを開発します")
}
​
val developer = new FrontendEngineer("遼子")
developer.greet()
developer.develop()
​
// OOP三原則
// 継承（is-aの関係を考えること）
// 多態性（振る舞いの結果が実体によって変わること）

```

## Object指向

```text
コンポーネント指向とオブジェクト指向
→部品再利用
柔軟な拡張と変更

---

オブジェクト＝もの・ことなど
オブジェクト指向＝オブジェクト同士の対話、でプログラムを設計すること
人間
車
「人間」が「車」を使う
「車」が「人間」を運ぶ

---

is-a, has-a関係

is-a
「動物」には「犬」と「猫」がいる
o「犬」は「動物」である。Dog is an Animal.
o「猫」は「動物」である。
x「動物」は「犬」である。Animal is a Dog.
x「犬」は「猫」である。
o「CategoryController」は「Controller」である。
x「Controller」は「CategoryController」である。

has-a
「車」
エンジン、タイヤ、ハンドル、窓、etc.
x「車」は「エンジン」である。
x「エンジン」は「車」である。
o「車」は「エンジン」を持っている。Car has a Engine.
o「車」は「ハンドル」を持っている。Car has a Handle.
o「CategoryController」は「CategoryRepository」を持っている。
x「CategoryRepository」は「CategoryController」を持っている。

```

## 値\(再代入不可\)

```scala
val x: Int = 1 + 1
println(x) // 2
```

## 変数

```scala
var x: Int = 1 + 1
x = 3 // "x"は"var"キーワードで宣言されているので、これはコンパイルされます。
println(x * x) // 9
```

## ブロック

```scala
println({
  val x = 1 + 1
  x + 1
}) // 3
```

## 関数（無名関数）

```scala
(x: Int) => x + 1
```

## 関数

```scala
val addOne = (x: Int) => x + 1
println(addOne(1)) // 2
```

## Method

{% embed url="http://www.ne.jp/asahi/hishidama/home/tech/scala/def.html" %}

メソッド本体にある最後の式はメソッドの戻り値になります。\(Scalaにはreturnキーワードはありますが、めったに使われません。\)

```scala
def addThenMultiply(x: Int, y: Int)(multiplier: Int): Int = (x + y) * multiplier
println(addThenMultiply(1, 2)(3)) // 9

def getSquareString(input: Double): String = {
  val square = input * input
  square.toString
}
println(getSquareString(2.5)) // 6.25
```

```scala
def myloop(n: Int)(f: => Unit) = {
      var m = n
      while (m > 0) {
       f
        m -= 1
      }
    }
     
// myloop: (n: Int)(f: => Unit)Unit

myloop(3){ println("abc") } 
// 関数（メソッド）を呼び出す際、引数が1個しかない引数リストでは、丸括弧を省略できる。
```

## Class

```scala
class Greeter(prefix: String, suffix: String) {
  def greet(name: String): Unit println(prefix + name + suffix)
}

val greeter = new Greeter("Hello, ", "!")
greeter.greet("Scala developer") // Hello, Scala developer!
```

Unitはvoidのように戻り値として意味がないことを示します。 （全てのScalaの式は値を持つ必要があるため、voidと違いUnit型のシングルトンで\(\)と書かれる値がありますが、その値には情報はありません。）

❓**ClassとMethodの用途の違いは？**

## Case Class

ケースクラスは、new キーワードなしでインスタンス化でき、値で比較されます。

```scala
case class Point(x: Int, y: Int)

val point = Point(1, 2)
val anotherPoint = Point(1, 2)
val yetAnotherPoint = Point(2, 2)

if (point == anotherPoint) {
  println(point + " と " + anotherPoint + " は同じです。")
} else {
  println(point + " と " + anotherPoint + " は異なります。")
} // Point(1,2) と Point(1,2) は同じです。
```

## Object

名前を参照してオブジェクトにアクセスすることができます。

```scala
object IdFactory {
  private var counter = 0
  def create(): Int = {
    counter += 1
    counter
  }
}

val newId: Int = IdFactory.create()
println(newId) // 1
val newerId: Int = IdFactory.create()
println(newerId) // 2
```

## Trait

トレイトはいくつかのフィールドとメソッドを含む型です。複数のトレイトを結合することもできます。

```scala
trait Greeter {
  def greet(name: String): Unit
}
```

{% embed url="https://docs.scala-lang.org/ja/tour/mixin-class-composition.html" %}



## Main Method

メインメソッドはプログラムの始点になります。Javaバーチャルマシーンはmainと名付けられたメインメソッドが必要で、 それは文字列の配列を一つ引数として受け取ります。

```scala
object Main {
  def main(args: Array[String]): Unit =
    println("Hello, Scala developer!")
}
```

## 文字列の補完

{% embed url="https://docs.scala-lang.org/ja/overviews/core/string-interpolation.html" %}



