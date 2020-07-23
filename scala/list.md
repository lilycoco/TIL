# List

## ::, +:

先頭に要素を追加

```scala
9 :: List(1,2,3)

List(9, 1, 2, 3)
```

## :::, ++

リストを結合

```scala
List(1,2,3) ::: List(4,5,6)

List(1, 2, 3, 4, 5, 6)
```

## slice

from以上until未満の要素のコレクションを返す。

```scala
List('a', 'b', 'c', 'd', 'e').slice(1,4)    

List('b', 'c', 'd')
```

## mkString

Stringを生成する。

```scala
	List(1,2,3).mkString("[", "-", "]")
	
	[1-2-3]
```

## map

```scala
List(2, 7, 1, 8).map(x => x + 1)
List(2, 7, 1, 8).map(_ + 1)

res: List(3, 8, 2, 9)
```

```scala
List(2, 7, 1, 8).map(_ > 2)

res: List(false, true, false, true)
```

```scala
List(2, 7, 1, 8).map(c => if (c > 2) 'a' else 'b')

res: List(b, a, b, a)
```

```scala
BToAMap.map { case (key, value) => (value, key)}
```

## zipWithIndex

インデックスを振る

```scala
List("a", "a", "a").zipWithIndex.map { case (item, index) => index + "番目の" + item }
```

## collect

filterとmapを合わせたようなもの。 caseにマッチした結果だけでコレクションが作られる。

```scala
List(1,2,3).collect{ case 1 => "one"; case 2 => "two" }

res: List(one, two)
```

```scala
List[Any]("a",1,"b",2).collect{ case s:String => s }

res: List[String](a, b)
```

