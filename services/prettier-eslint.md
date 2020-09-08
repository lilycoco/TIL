# Prettier, eslint

{% embed url="https://prettier.io/docs/en/options.html" %}

{% embed url="https://qiita.com/soarflat/items/06377f3b96964964a65d" %}

{% embed url="https://qiita.com/soarflat/items/d583356e46250a529ed5" %}

`eslint`と`prettier`を併用する場合、以下のように役割分担すると良い

* `eslint`: 構文チェック
* `prettier`: コード整形

`.eslintrc`の`extends`に`prettier`を追加することで併用出来る



