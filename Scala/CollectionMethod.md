# Collection Method

## transpose

transpose メソッドは、二重の Seq オブジェクトに対して呼び出せ、二重の Seq を行列と見做したときの転置行列（に対応する二重 Seq）を返します。

```scala
  val metaSeq = Seq(
    Seq(1, 2, 3, 4),
    Seq(5, 6, 7, 8),
    Seq(9, 10, 11, 12))

  // transpose メソッド
  assert( metaSeq.transpose ==
    Seq(
      Seq(1, 5, 9),
      Seq(2, 6, 10),
      Seq(3, 7, 11),
      Seq(4, 8, 12)) )
```

## flatten

flatten メソッドは二重の Seq に対して呼び出せ、内側の Seq をバラして一重の Seq にします。

```scala
  val metaSeq = Seq(
    Seq(1, 2, 3, 4),
    Seq(5, 6, 7, 8),
    Seq(9, 10, 11, 12))

  // flatten メソッド
  assert( metaSeq0.flatten == Seq(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12) )
```

## flatMap

flatMap メソッドは、map メソッドと flatten メソッドを併せて行うようなメソッドです。　
レシーバとなる Seq オブジェクトは一重の Seq で構いません。　
引数として、各要素を任意の要素型の Seq オブジェクトに変換する関数をとり、返り値はそれらの関数を適用した結果の Seq を（flatten を呼び出したように）平坦化します。

```scala
  val intSeq = Seq(1, 2, 3)

  // flatMap メソッド
  val result = intSeq.flatMap(i => Seq.fill(4)(i))  // 同じ値を4つ要素とする Seq を返す
  assert( result == Seq(1, 1, 1, 1, 2, 2, 2, 2, 3, 3, 3, 3) )

  // flatMap は map & flatten と同じ
  val f: Int => Seq[Int] = i => Seq.fill(4)(i)
  assert( intSeq.map(f).flatten == intSeq.flatMap(f) )
```
