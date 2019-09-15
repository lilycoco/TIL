# Twilio

## 電話番号規制についての詳細
https://twilio.kddi-web.com/phone-number_regulatory/


## Proxy 匿名電話に特化
[Twilio Proxy ベータ版リリースのご案内](https://twilio.kddi-web.com/dev/691/)
> Proxy を使うと数行のコードで複数の異なるコミュニケーションチャネルをブリッジできるようになります。ただブリッジするだけではなく、互いの個人情報を伏せたり、互いが接続する時間の長さをコントロールしたり、互いが自分自身のデバイスを使いつつ互いが何を共有するのか制限することが可能です。

[Doc / Proxyクイックスタート](https://jp.twilio.com/docs/proxy/quickstart#make-a-voice-call)

[Qiita / Twilio Proxyの使い方](https://qiita.com/ManabuMiwa/items/1e17a4d428f477ef1c3b)  

> Sessionsには有効期限 (DateExpiry) や生存時間 (TTL) を設けることができ、これを過ぎるとSessionsのStatusプロパティーがClosedとなり、両者でのコミュニケーションは行えなくなります。

> modeというオプションのパラメーターはSessionで使用できるコミュニケーションのチャンネルを指定するものですが、既定値では`voice-and-message`となっており、音声通話とSMSを両方使用できることを意味しています。 ところが日本のTwilio番号はSMSの送受信に対応しないため、既定値のままではそのSessionのParticipant追加の段階で「割り当てられるTwilio番号の候補がない」という旨のエラーが発生してしまいます。
これを防ぐには、このmodeパラメーターに明示的に`voice-only`という値を指定して、このSessionでは音声通話のみを許可するようにします。

```
    public function createSession()
    {
        list($twilio,) = $this->getId();
        $service = $this->getService();
        $session = $twilio->proxy->v1
            ->services($service->sid)
            ->sessions
            ->create(array(
                "uniqueName" => "MyFirstSess",
                "mode" => 'voice-only'
                ""
            ));
        dump($session);
    }
```
[Qiita / 複数の電話番号に電話をかける。](https://qiita.com/joohounsong/items/36da4e67b1652c60bf57)

## Conference

> V字発信の際のカンファレンス機能の必要性について、1対1の場合であれば必須ではない。しかし、通知メッセージを流したい場合、例えば通話終了3分前に自動音声で延長の確認を取るなどはカンファレンス機能が必要。

[Qiita / Twilioを使って、二者間の通話に別の音声を挿入する](https://qiita.com/mobilebiz/items/4490fe5a03c5192ce06f)

