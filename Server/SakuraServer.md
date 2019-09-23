# さくらサーバー

▼環境設定の手順

[https://neenee.sakura.ne.jp/](https://neenee.sakura.ne.jp/?fbclid=IwAR33gJZ-F0evaclfatPISHOyKix5pB4ONud0fRaI0KLVqhhVM_DmiqK5MUY) 

■さくらにアップするまでの手順

[【 超簡易的 】githubにpushで、さくらサーバーに自動デプロイ](https://qiita.com/prex-uchida/items/f8bc05eb91b944b6214e) 

## さくらインターネット　サーバー登録

[ステージング環境](https://www.notion.so/a1d19833a4cd42c0aabafcf363fbda18)

さくらサーバーログイン

```
ssh tanbobcat4@tanbobcat4.sakura.ne.jp
ssh neenee_stg
```
[本番環境](https://www.notion.so/23a184ebd16b4f0db977e0867e47936d)

```
ssh neenee@neenee.sakura.ne.jp
ssh neenee_master
```
## さくらサーバーにgitをインストール

    さくらインターネットのレンタルサーバーへGitをソースコードからインストールする手順

    [https://itlogs.net/sakura-internet-git-install/](https://itlogs.net/sakura-internet-git-install/) 

## さくらからgithubへのSSH接続の設定（Githubに公開鍵、サーバー側に秘密鍵を設置）

[GitHubでssh接続する手順~公開鍵・秘密鍵の生成から~ （サーバーにログイン後に鍵を生成）](https://qiita.com/shizuma/items/2b2f873a0034839e47ce) 

[さくらレンタルサーバーにSSH＆公開鍵で接続してみよう！　(ローカルとの接続)](http://vdeep.net/sakura-ssh) 

[Githubに接続できない時の対処法](https://qiita.com/nyanchu/items/32d65c4c36315b876d38) 

[SSHなるものをよくわからずに使っている人のための手引書](https://qiita.com/kenju/items/b09199c4b3e7203a2867) 

scp id_rsa.pub tanbobcat4@tanbobcat4.sakura.ne.jp:/home/tanbobcat4/.ssh/authorized_keys

## サーバー上で表示

[さくらレンタルサーバーにLaravelを入れてとりあえず動くようにした話](https://qiita.com/tosite0345/items/aa3ba6fa4387c4c649a8) 

[ローカルにある Laravel を公開サーバーにデプロイ](https://laraweb.net/environment/3192/) 

[GitHubからさくらのレンタルサーバに自動デプロイ](https://qiita.com/su_/items/ef40b9d9f7fdc99e3b16) 

> 記事でいうところの deploy.php をレンサバの例えば /home/tanbobcat4/www/ ｀webhook｀ /deploy.phpに設置し、github から叩いてもらうという感じです。github が webhook で叩きに来る際の 接続元IPなどが公開されているのであれば webhook ディレクトリに接続元制限をかけたいところでありますが。

[さくらレンタルサーバにLaravelをインストールする](https://laraweb.net/environment/674/) 

[git push + さくらのレンタルサーバーで github pages ライクにデプロイする方法](http://d.hatena.ne.jp/punitan/20110709/1310192132) 

[Bitbucket や GitHub で自動デプロイするためのサンプル PHP スクリプトを拾って改造してみた](http://ja.katzueno.com/2015/01/3390/) 

[Gitでサイトを更新したい！](https://qiita.com/dojineko/items/b11d279d1ff8cfacf3dc) 

## Webhook

[Webhookとは？](https://qiita.com/soarflat/items/ed970f6dc59b2ab76169)
