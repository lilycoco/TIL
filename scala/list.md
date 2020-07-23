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

