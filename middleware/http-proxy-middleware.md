# http-proxy-middleware

[https://github.com/chimurai/http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware)

* `proxy('**', {...})` matches any path, all requests will be proxied.
* `proxy('**/*.html', {...})` matches any path which ends with `.html`
* `proxy('/*.html', {...})` matches paths directly under path-absolute
* `proxy('/api/**/*.html', {...})` matches requests ending with `.html` in the path of `/api`
* `proxy(['/api/**', '/ajax/**'], {...})` combine multiple patterns
* `proxy(['/api/**', '!**/bad.json'], {...})` exclusion

> two code as below give same results

**src/setupProxy.js**

```text
const proxy = require('http-proxy-middleware')

module.exports = function(app) {
    app.use(proxy(['/api', '/auth/google'], { target: 'http://localhost:5000' }));
}
```

```text
const proxy = require('http-proxy-middleware');

module.exports = function(app) {
    app.use(proxy('/auth/google', { target: 'http://localhost:5000' }));
    app.use(proxy('/api/**', { target: 'http://localhost:5000' }));
};
```

## References

[知っていると捗るcreate-react-appの設定](https://qiita.com/geek_duck/items/6f99a3da15dd39658fff)

