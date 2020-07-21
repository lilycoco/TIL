# Pattern of rerender

## requestAnimationFrame

ブラウザの描画タイミング毎 に実行

* タブがバックグラウンドになった時に fps を落として実行される
* ブラウザの描画更新単位と同じ単位で呼び出される

### Pros

* 低メモリ消費
* 省電力
* アニメーションに適している

## setinterval

* 指定時間毎に実行

