# conf

## conf/application.conf

ここに今書いているのはローカル開発環境のID/PWやキーだから、ローカル上に立ち上がってるコンテナで利用されているもので、開発者間で共有しないとむしろ面倒😇

local、dev、stg、prodって今後環境が増えたとしたら、

* application.conf.local
* application.conf.dev
* application.conf.stg
* application.conf.prod

って用意して起動された環境に応じて読み込むconfファイルを切り替えるってのがよく使われる手法ですね👽  
AWSを利用する場合はパラメータストアを組み合わせたりして設定を隠したりもするね🙌  
オンプレでデプロイをしていた頃はサーバに直接入って直接編集したりとかしてたね😥

{% embed url="https://docs.aws.amazon.com/ja\_jp/systems-manager/latest/userguide/systems-manager-parameter-store.html" %}



