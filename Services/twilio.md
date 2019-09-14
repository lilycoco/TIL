# Twilio

V字発信の際のカンファレンス機能の必要性について、1対1の場合であれば必須ではない。
しかし、通知メッセージを流したい場合、例えば通話終了3分前に自動音声で延長の確認を取るなどはカンファレンス機能が必要。
https://qiita.com/mobilebiz/items/4490fe5a03c5192ce06f

## 電話番号規制についての詳細
https://twilio.kddi-web.com/phone-number_regulatory/

## 匿名に特化したProxy
・https://qiita.com/ManabuMiwa/items/1e17a4d428f477ef1c3b
・https://twilio.kddi-web.com/dev/691/

## 複数の電話番号に電話をかける。
https://qiita.com/joohounsong/items/36da4e67b1652c60bf57

• v字発信の実現
• 1つの電話番号で運用するために、conferenceのroomを動的に生成
•申し込みしたユーザーとゲイのroomを動的に割り振り
• 通話時間の取得
•hostテーブルに「オンラインステータス」のカラム追加（ 選択肢：「離席中」「通話中」「すぐに話せる」）
•hostテーブルに「オンライン時間」のカラム追加
•ホスト管理画面に「オンライン中」にするボタンと「何時までオンライン状態にするか」を設定するプルダウンを追加
•hostテーブルのオンラインステータスの変更処理
•申し込みから10分以内に通話開始しないor「キャンセルボタン」を押した時のキャンセル処理の実装。キャンセルした場合、roomの割り振りを取り消して、v時発信もしないようにする
