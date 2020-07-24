# List



```scala
head : 先頭
last : 末尾
init : 末尾以外
tail : 先頭以外
slice : 範囲指定
```

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

## zip

2つのコレクションの同じ位置の要素をペアにしたコレクションを返す。 長さは短い方に合わせられる。

```scala
List(1,2,3,4).zip(List('a','b','c'))

res: List((1,'a'), (2,'b'), (3,'c'))
```

## partition

条件を満たすコレクションと満たさないコレクションを返す。

```scala
List(1,2,3,4,5).partition(n => n%2==1)

res: (List(1, 3, 5),List(2, 4))
```

```scala
def findOutlier(integers: List[Int]): Int =
  integers.partition(_%2 == 0) match {
    case (List(outlier), _) => outlier
    case (_, List(outlier)) => outlier
  }
  
findOutlier([2, 4, 0, 100, 4, 11, 2602, 36])
// 11
```

## sorted

```scala
List(2,1,4,3).sorted

res: List(1, 2, 3, 4)
```

## sortWith

関数ltで比較・ソートしたコレクションを返す。

```scala
List("a2","b1","c4", "d3").sortWith((s1,s2) => s1(1) < s2(1))
List("a2","b1","c4", "d3").sortWith(_.charAt(1) < _.charAt(1))
List("a2","b1","c4", "d3").sortWith(_.apply(1) < _.apply(1))
List("a2","b1","c4", "d3").sortWith(_(1) < _(1))

res: List(b1, a2, d3, c4)
```

## sortBy

要素AをBに変換し、Bでソートされたコレクションを返す。

```scala
List("a2","b1","c4", "d3").sortBy(s => s(1))

res: List(b1, a2, d3, c4)
```

## until

```scala
(1L until number)
\\ Long typeでリストを作る
```

