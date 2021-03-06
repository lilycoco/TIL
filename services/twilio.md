# Twilio

## Index

* [目的](twilio.md#目的)  
  * [条件](twilio.md#条件)  
* [Set up](twilio.md#set-up)
* [Twilioを介しての通話](twilio.md#Twilioを介しての通話)
* [Proxy](twilio.md#Proxy)
* [V字発信 Conference使用](twilio.md#V字発信-Conference使用)
* [V字発信 Conference不使用](twilio.md#V字発信-Conference不使用)
* [TwiML 動的生成](twilio.md#TwiML-動的生成)
* [料金](twilio.md#料金)
* [電話番号規制について](twilio.md#電話番号規制について)
* [References](twilio.md#references)

  **目的**

  不特定多数の二者間通話を実現する

  **条件**

* 合計費用を出来るだけ安くする
* １つの電話番号のみ使用
* 通話にかかる費用を管理するため、個別にかかるキャリア通信料をなるべく発生させない。（Twilioにかかる料金のみに止める）
* ログを取得する\(通話開始、終了、通話時間\)

## Set up

Twilioのライブラリをインストールする

```text
composer require twilio/sdk
```

[The Twilio PHP ヘルパーライブラリー](https://jp.twilio.com/docs/libraries/php)

## Twilioを介しての通話

> Twilioで取得した電話番号に電話すると、相手の電話番号に転送される

_**Pros**_

* シンプル

_**Cons**_

* 複数の電話番号が必要  
* 発信者にキャリア通信料がかかる  
* Wbhookに登録する番号を動的に変更するやり方が不明  

**以下のXMLをTwilioで取得した電話番号のWebhookに登録する**

```markup
  <?xml version="1.0" encoding="UTF-8"?>
  <Response>
      <Dial timeout="20" callerId="+81 [twilioで取得した電話番号]">
        +81 [転送先電話番号]
      </Dial>
  </Response>
```

## Proxy

> 匿名電話に特化しており、１つの電話番号で複数のSessionを作ることが出来、同じSessionに登録されたParticipantの片方がSessionが作成された電話番号にかけるともう一方のParticipantと通話ができる。互いの個人情報を伏せたり、接続する時間の長さをコントロールすることが可能。

_**Pros**_

* 電話番号の扱いが簡単で、call back urlがなくても実装可能。ログも取得できる。    
* １つの電話番号で実装可能。  

_**Cons**_

* 別途Proxy料金がかかる。  
* V字発信出来ない？  

[Twilio Proxy ベータ版リリースのご案内](https://twilio.kddi-web.com/dev/691/)

[Doc / Proxyクイックスタート](https://jp.twilio.com/docs/proxy/quickstart#make-a-voice-call)

[Qiita / Twilio Proxyの使い方](https://qiita.com/ManabuMiwa/items/1e17a4d428f477ef1c3b)

**オプションパラメーター\(Sessions\)**

> 有効期限 \(DateExpiry\) や生存時間 \(TTL\) を過ぎるとSessionsのStatusプロパティーがClosedとなり、両者でのコミュニケーションは行えなくなります。
>
> mode: コミュニケーションのチャンネルを指定するもの。既定値では`voice-and-message`となっており、音声通話とSMSを両方使用できることを意味しています。 ところが日本のTwilio番号はSMSの送受信に対応しないため、既定値のままではそのSessionのParticipant追加の段階で「割り当てられるTwilio番号の候補がない」という旨のエラーが発生してしまいます。 これを防ぐには、このmodeパラメーターに明示的に`voice-only`という値を指定して、このSessionでは音声通話のみを許可するようにします。

```php
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
    }
```

**Interactions**

> Interactionsは、Session内でParticipantsがお互いに交わした通話やメッセージングの個々の履歴を表す、読み取り専用のリソースです。

```php
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

## V字発信 Conference使用

> Twilioから複数の番号に一斉に電話をかけ、同じカンファレンスルームにリダイレクトさせる。 V字発信へカンファレンス機能は必須ではないが、通知メッセージを流したい場合など（例えば、自動音声で通話終了3分前に延長の確認をしたい場合）はカンファレンス機能が必要。

[Twilioカンファレンス機能（同時通話）](https://twilio.kddi-web.com/dev/636/)

_**Pros**_

* １つの電話番号のみで実装可能。  
* 両者へキャリア通信料がかからない。  

_**Cons**_

* 別途カンファレンス料金がかかる。  
* call back urlが必要。実装がProxyに比べ大変？ 

```php
    public function multipleCall($roomName = null)
    {
        list($twilio,) = $this->getId();
        $callNumbers = array(env('CALL_NUMBER'), env('CALL_NUMBER_2'));
        foreach ($callNumbers as $Number) {
            $call = $twilio->account->calls->create(
                $Number,
                env('TWILIO_NUMBER'),
                array("url" => env('TWIML_BIN_FOR_CONFERENCE') . "?room=" . $roomName)
            );
        }
    }
```

[Making Calls](https://www.twilio.com/docs/voice/make-calls)  
[Qiita / 複数の電話番号に電話をかける。](https://qiita.com/joohounsong/items/36da4e67b1652c60bf57)

**TWIML\_BIN\_FOR\_CONFERENCE**

```markup
    <?xml version="1.0" encoding="UTF-8"?>
    <Response>
      <Dial>
        <Conference>{{room}}</Conference>
      </Dial>
    </Response>
```

[TwiML Bins](https://www.twilio.com/docs/runtime/tutorials/twiml-bins)

## V字発信 Conference不使用

> Twilioから片方の電話番号にかけ、受信後すぐに相手の電話番号へかける。

_**Pros**_

* １つの電話番号のみで実装可能\(?\)。←要確認    
* 両者へキャリア通信料がかからない。  
* 別途カンファレンス料金がかからない。  

_**Cons**_

* call back urlが必要。実装がProxyに比べ大変？ 

```php
    public function makeACall($callNumber = null)
    {
        list($twilio,) = $this->getId();
        $call = $twilio->account->calls->create(
            env('CALL_NUMBER'),
            env('TWILIO_NUMBER'),
            array("url" => env('TWIML_BIN_FOR_CALL_FOWARDING') . "?number=" . $callNumber)
        );
    }
```

**TWIML\_BIN\_FOR\_CALL\_FOWARDING**

```markup
    <?xml version="1.0" encoding="UTF-8"?>
    <Response>
        <Dial>
           {{number}}
        </Dial>
    </Response>
```

## TwiML 動的生成

[twilio-php](https://jp.twilio.com/docs/libraries/reference/twilio-php/)

[TwilioSDKで翻弄されています](https://hisa-tech.site/2019/05/3098/)

[PHPおよびLaravelを使用したワンクリック通話](https://jp.twilio.com/docs/voice/tutorials/click-to-call-php-laravel)

## 料金

Aさんの電話--------&gt; Twilio ---------&gt;Bさんの電話

上記のような場合、AさんからTwilioが着信、TwilioからBさんが発信となります。 Twilioアカウント保持者は着信料と発信料をお支払いただく形になります。

* 着信料: 固定・携帯電話共に1分1円
* 発信料: 固定宛の場合5.4円/分、携帯宛の場合16.2円/分
* 別途Aさんはキャリアによる通話料が発生

## 電話番号規制について

[https://twilio.kddi-web.com/phone-number\_regulatory/](https://twilio.kddi-web.com/phone-number_regulatory/)

## References

[Qiita / Twilioを使って、二者間の通話に別の音声を挿入する](https://qiita.com/mobilebiz/items/4490fe5a03c5192ce06f)

