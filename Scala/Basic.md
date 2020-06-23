## 値(再代入不可)

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
メソッド本体にある最後の式はメソッドの戻り値になります。(Scalaにはreturnキーワードはありますが、めったに使われません。)

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
```
Unitはvoidのように戻り値として意味がないことを示します。
（全てのScalaの式は値を持つ必要があるため、voidと違いUnit型のシングルトンで()と書かれる値がありますが、その値には情報はありません。）



