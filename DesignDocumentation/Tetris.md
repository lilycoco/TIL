# Tetris

## State がSetされるタイミング：

1. 1秒ごとにTimerを叩く時
2. 方向キー入力が行われた時
3. Start, Stop ボタンが押された時

---

### 1. 　1秒ごとにTimerを叩く時 :

現状の状態を判定して、Stateを変える

if (running  が true である) {

Timerをたたくく

If （もう下にいけない）{

if (積み上げたブロックが上まで到達している) {

- Game Overを表示

}

else {

- newBoard のブロックをBoardに転写

    If (ブロックが揃った行がある）{

    - その行を消去する

    }

- 新しいブロックを生成する（位置を初期化、色、形を変更）

}

else

- ブロックの位置を下に移動する

}

else {

Timerをたたかない

}

---

### 2. 　方向キー入力が行われた時:

現状の状態を判定して、Stateを変える

- running  を false にする

    If （まだ左右下に余地がある）{

    - （左右下のキーを入力）キーごとにブロックの位置を変更する
    - （回転キーを入力）ブロックの形を変更する

    }

- running  を true にする

---

### 3. 　Start, Stop ボタンが押された時

if (running が true である) {

- running  を false にする

}

else {

- running  を true にする

}
