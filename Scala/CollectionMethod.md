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
