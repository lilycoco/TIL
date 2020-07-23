# String

## replaceAll

第一引数に[正規表現](http://d.hatena.ne.jp/keyword/%C0%B5%B5%AC%C9%BD%B8%BD)を受け取り、置き換える

```scala
println( "abc_abc".replaceAll( "[a-z]+", "123" ) );
// "123_123"
```

