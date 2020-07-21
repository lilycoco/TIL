# SSH認証

秘密鍵のファイル名をデフォルトから変更した場合は、 `config` ファイルに以下を設定。  
`ssh test`でログイン出来る

```text
Host test
  User userName
  HostName test.jp
  Port 22
  IdentityFile ~/.ssh/id_test_rsa
  ServerAliveInterval 60
```

[さくらレンタルサーバーにSSH＆公開鍵で接続してみよう！](http://vdeep.net/sakura-ssh)  
[あなたが現在インターネットに接続しているグローバルIPアドレス確認](https://www.cman.jp/network/support/go_access.cgi)

