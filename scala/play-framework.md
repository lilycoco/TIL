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

```text
├── app アプリケーションのソースコード
│   ├── controllers アプリケーションのコントローラ
│   ├── views テンプレート
│   └── services (option)
├── build.gradle
├── build.sbt アプリケーションのビルドスクリプト, Build用のライブラリ設定
├── conf アプリケーションの設定ファイル
│   ├── application.conf メイン設定ファイル, データベースの設定
│   ├── logback.xml ログ出力設定ファイル
│   ├── messages 国際化対応用言語ファイル
│   ├── routes ルート定義
│   └── evolutions evolutions設定　(option)
│       └── default 
│           └── 1.sql SQL
├── gradle
├── gradlew
├── gradlew.bat
├── project sbt 設定ファイル群
│   ├── build.properties sbt プロジェクトの目印
│   ├── plugins.sbt Play 自身の定義を含む sbt プラグイン
│   ├── project
│   ├── scaffold.sbt scaffolding 用の sbt プラグイン使用時に使用
│   └── target
├── public 公開アセット
├── target ビルド成果物
└── test 単体、および機能テスト用のソースフォルダ
```

## Database

### evolutions

DBマイグレーションツール

スキーマのバージョン管理のようなもの。スキーマの変更に対してデータベースを追随させることができる。evolutionsはPlayframeworkで使われているDBマイグレーションツール

### Anorm

SQLの実行/結果解析をサポートする、データベースアクセスのためのライブラリ







