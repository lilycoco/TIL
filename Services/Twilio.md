# Twilio

## 電話番号規制についての詳細
https://twilio.kddi-web.com/phone-number_regulatory/


## Proxy (匿名電話に特化)
[Twilio Proxy ベータ版リリースのご案内](https://twilio.kddi-web.com/dev/691/)
> Proxy を使うと数行のコードで複数の異なるコミュニケーションチャネルをブリッジできるようになります。ただブリッジするだけではなく、互いの個人情報を伏せたり、互いが接続する時間の長さをコントロールしたり、互いが自分自身のデバイスを使いつつ互いが何を共有するのか制限することが可能です。

[Doc / Proxyクイックスタート](https://jp.twilio.com/docs/proxy/quickstart#make-a-voice-call)

[Qiita / Twilio Proxyの使い方](https://qiita.com/ManabuMiwa/items/1e17a4d428f477ef1c3b)  

**オプションパラメーター(Sessions)**

> 有効期限 (DateExpiry) や生存時間 (TTL) を過ぎるとSessionsのStatusプロパティーがClosedとなり、両者でのコミュニケーションは行えなくなります。

> mode: コミュニケーションのチャンネルを指定するもの。既定値では`voice-and-message`となっており、音声通話とSMSを両方使用できることを意味しています。 ところが日本のTwilio番号はSMSの送受信に対応しないため、既定値のままではそのSessionのParticipant追加の段階で「割り当てられるTwilio番号の候補がない」という旨のエラーが発生してしまいます。
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
                "date_expiry" => "2015-07-30T20:00:00Z",
                "ttl"=> 3600,
                "mode" => 'voice-only'
            ));
        dump($session);
    }
```

**Interactions**
> Interactionsは、Session内でParticipantsがお互いに交わした通話やメッセージングの個々の履歴を表す、読み取り専用のリソースです。

```
    public function getInteraction()
    {
        list($twilio,) = $this->getId();
        $service = $this->getService();
        $session = $this->getSession();
        $interactions = $twilio->proxy->v1
            ->services($service->sid)
            ->sessions($session->sid)
            ->interactions
            ->read(array(), 20);

        foreach ($interactions as $record) {
            dump($record); // ($record->data) call duration
        }
    }
```


## Conference

> V字発信の際、1対1の場合であればのカンファレンス機能は必須ではない。しかし、通知メッセージを流したい場合、例えば通話終了3分前に自動音声で延長の確認を取る場合などはカンファレンス機能が必要。

[Twilioカンファレンス機能（同時通話）](https://twilio.kddi-web.com/dev/636/)  

[Qiita / 複数の電話番号に電話をかける。](https://qiita.com/joohounsong/items/36da4e67b1652c60bf57)

> 複数の番号に一斉に電話をかけ、同じカンファレンスルームにリダイレクトさせる

```
    public function multipleCall()
    {
        list($twilio,) = $this->getId();
        $callNumbers = array(env('CALL_NUMBER'), env('CALL_NUMBER_2'));
        foreach ($callNumbers as $Number) {
            $call = $twilio->account->calls->create(
                $Number,
                env('TWILIO_NUMBER'),
                array("url" => env('CALL_BACK_URL_FOR_CONFERENCE'))
            );
        }
        dump($call);
    }
```
**CALL_BACK_URL_FOR_CONFERENCE**
```
    <?xml version="1.0" encoding="UTF-8"?>
    <Response>
      <Dial>
        <Conference>My conference</Conference>
      </Dial>
    </Response>

```

## V字発信（カンファレンス不使用）

> 片方の電話番号にかけ、受信後すぐに相手の電話番号へかける。両者へTwilioからの発信となり、キャリア通信料がかからない

```
    public function makeACall()
    {
        list($twilio,) = $this->getId();
        $call = $twilio->account->calls->create(
            env('CALL_NUMBER'),
            env('TWILIO_NUMBER'),
            array("url" => env('CALL_BACK_URL_FOR_CALL_FOWARDING'))
        );
        dump($call);
    }
```
**CALL_BACK_URL_FOR_CALL_FOWARDING**
```
    <?xml version="1.0" encoding="UTF-8"?>
    <Response>
        <Dial>
            <Number>
                +81XXXXXXXXX
            </Number>
        </Dial>
    </Response>
```

## 料金

Aさんの電話--------> Twilio --------->Bさんの電話

上記のような場合、AさんからTwilioが着信、TwilioからBさんが発信となります。
Twilioアカウント保持者は着信料と発信料をお支払いただく形になります。

- 着信料: 固定・携帯電話共に1分1円
- 発信料: 固定宛の場合5.4円/分、携帯宛の場合16.2円/分
- 別途Aさんはキャリアによる通話料が発生

## References

[Qiita / Twilioを使って、二者間の通話に別の音声を挿入する](https://qiita.com/mobilebiz/items/4490fe5a03c5192ce06f)
