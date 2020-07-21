# npm

## 環境変数を指定してポートを立ち上げる

`cross-env PORT=[8000] npm start`

```text
[package.json]

"scripts": {
  "start": "next start -p %PORT%"
}
```

