# String

## replaceAll

第一引数に[正規表現](http://d.hatena.ne.jp/keyword/%C0%B5%B5%AC%C9%BD%B8%BD)を受け取り、置き換える

```scala
"abc_abc".replaceAll("[a-z]+", "123")
// "123_123"

"  123  _ 123 ".replaceAll(" {2,}", " ").trim
// "123 _ 123"

"name=john age=13 year=2001".replaceAll("\\s+","")
// "name=johnage=13year=2001"
```

## toLowerCase

小文字変換

```scala
"WINDOWS".toLowerCase
// windows
```

## toUpperCase

大文字変換

```scala
"Linux".toUpperCase 
// LINUX
```

## capitalize

文字列の先頭を大文字にする。先頭が英字でなければそのままの値を返す。

```scala
"mac" capitalize
// Mac
```

## isLetter

文字かどうかを返す

## asDigit

```scala

'E'.asDigit 

```

## groupBy

同じ値でまとめる

```scala
val s = Seq("apple", "oranges", "apple", "banana", "apple", "oranges", "oranges")
s.groupBy(identity).mapValues(_.size)

res: Map(banana -> 1, oranges -> 3, apple -> 3)

s.groupBy(identity).mapValues(_.size)("apple")
// 3
```

## split

```scala
"a,b,c".split(",")

res: Array[String] = Array(a, b, c)
```

## 文字列の補完

```scala
val foo = "bar"
s"abc $foo def ${ 1+2 }"
== "abc " + foo + " def " + (1+2)
```

