# XAMPP

[Laravel ローカル環境構築（for XAMPP） ](https://laraweb.net/tutorial/3171/)

[xamppでLaravelの環境構築](https://qiita.com/maruyama42/items/43d7029d7e00e587bf0b)

[Mac OSにLaravelローカル開発環境構築](https://qiita.com/da-sugi/items/7ee7a458aad4209bab01)

## Xamppバージョンアップ

[https://kde.hateblo.jp/entry/2018/02/12/022840](https://kde.hateblo.jp/entry/2018/02/12/022840)

> PHPは VC15 x86 Thread Safeを選択

## 【PHP】PHPをインストールしたらやっておきたい設定

[https://qiita.com/knife0125/items/0e1af52255e9879f9332](https://qiita.com/knife0125/items/0e1af52255e9879f9332)

```text
✖ memory_limit = 32MB 

〇 memory_limit = 32M
```

追加で下記有効にする

```text
extension=fileinfo 

extension=openssl 

extension=php_mbstring.dll
```

> うまくいかない時はXamppごとインストールしなおした方が早いかも

ポート80番が使えない場合は `netstat -nao` でPIDを確認、tast を終了する

## composer install

## Laravel5.5 データベース　マイグレーション

`php artisan migrate:refresh --seed`

[【Laravel】マイグレーションの動作について ](https://www.out48.com/archives/3406/)

[はじめての MySQL](https://qiita.com/unbabel/items/b784459356686641dabe)

[ユーザ権限の確認・追加](https://qiita.com/pinohara/items/481c95dc4c8c2568bf8d)

[MySQL5.7インストール後にrootでログインする方法](https://qiita.com/fujiiiiii/items/c30153179487a656d672)

[MySQL5.7の初期設定まとめ](https://qiita.com/ritukiii/items/08df5be6d50871124aaf)

[MySQLのrootパスワードを忘れた！ってときの対処法\(Windows\)](https://qiita.com/Nekonecode/items/c44896105f1c2b22630e)

[MySQL](https://dev.mysql.com/doc/refman/5.6/ja/resetting-permissions.html#resetting-permissions-windows)

[https://readouble.com/laravel/5.5/ja/migrations.html](https://readouble.com/laravel/5.5/ja/migrations.html)

## SSLを有効にする

[XAMPP for WindowsでSSLを有効にする](https://qiita.com/sutara79/items/21a068494bc3a08a4803)

## ローカルでの変更を別デバイスで確認

Ipconfig

Ex\)

IPv4 アドレス . . . . . . . . . . . .: 192.168.100.105

