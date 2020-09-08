# Basic

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

## Class

```scala
class Greeter(prefix: String, suffix: String) {
  def greet(name: String): Unit =
    println(prefix + name + suffix)
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

