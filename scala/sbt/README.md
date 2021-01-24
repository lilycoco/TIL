---
description: >-
  scala のビルドツールのこと
  scalaのコードだけでもscalacコマンドを使用したりすることでアプリケーションを生成出来るが、sbtを使うとモジュールやバージョン管理がしやすい
---

# sbt

## Basic Command

### sbt

作成したプロジェクト直下でsbtコマンドを実行するとsbtシェルに入り、sbtなしで様々なコマンドを実行することができます。

```text
$ sbt
```

### 実行

```text
> run

scalaシェルに入っていない場合
$ sbt run
```

### ビルド定義の再読み込み

```text
> reload
```

### バージョン確認

```text
> sbtVersion
```

### サーバー停止

`Ctrl + D`

データベース設定などを変更した場合は一度サーバーを停止し、`reload`, `run` する

## REPL

```text
> console

scala>
```





