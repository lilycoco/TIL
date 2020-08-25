# Play framework

## 環境構築

[Play Frameworkハンズオン環境構築](https://qiita.com/yuichi0301/items/4785e3fe490736d4ee50#3intellij%E7%92%B0%E5%A2%83%E6%A7%8B%E7%AF%89)

[Play Frameworkハンズオン](https://qiita.com/yuichi0301/items/ead86d0251b954f07935#1evolutions%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)

* [Homebrew](http://brew.sh/index_ja.html) 
* Java
* IntelliJ \(Ultimate版 はライセンスが必要なためCommunity版  を使用する\)
* sbt

## Directory

[Anatomy of a Play application](https://www.playframework.com/documentation/2.8.x/Anatomy)

### Routes

`/conf/routes`

### Controllers

`/app/controllers/TodoCoutroller.scala`

### Views

`/app/views/list.scala.html`

### Services

`/app/services/Todo.scala`

### Build

`/build.sbt`

Build用のライブラリ

### Database

`/conf/application.conf`

データベースの設定

### SQL

`/conf/evolutions/default/1.sql`

## Database

### evolutions

DBマイグレーションツール

スキーマのバージョン管理のようなもの。スキーマの変更に対してデータベースを追随させることができる。evolutionsはPlayframeworkで使われているDBマイグレーションツール

### Anorm

SQLの実行/結果解析をサポートする、データベースアクセスのためのライブラリ







