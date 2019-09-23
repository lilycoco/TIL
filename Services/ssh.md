# SSH認証

公開鍵のファイル名をデフォルトから変更した場合は、 `config` ファイルに以下を設定。  
`ssh test`でログイン出来る

```
Host test
  User userName
  HostName test.jp
  Port 22
  IdentityFile ~/.ssh/id_test_rsa
  ServerAliveInterval 60
```

[さくらレンタルサーバーにSSH＆公開鍵で接続してみよう！](http://vdeep.net/sakura-ssh)

