# http-proxy-middleware
https://github.com/chimurai/http-proxy-middleware


- `proxy('**', {...})` matches any path, all requests will be proxied.
- `proxy('**/*.html', {...})` matches any path which ends with `.html`
- `proxy('/*.html', {...})` matches paths directly under path-absolute
- `proxy('/api/**/*.html', {...})` matches requests ending with `.html` in the path of `/api`
- `proxy(['/api/**', '/ajax/**'], {...})` combine multiple patterns
- `proxy(['/api/**', '!**/bad.json'], {...})` exclusion


> two code as below give same results

```
const proxy = require('http-proxy-middleware')
 
module.exports = function(app) {
    app.use(proxy(['/api', '/auth/google'], { target: 'http://localhost:5000' }));
}
```

```
const proxy = require('http-proxy-middleware');
 
module.exports = function(app) {
    app.use(proxy('/auth/google', { target: 'http://localhost:5000' }));
    app.use(proxy('/api/**', { target: 'http://localhost:5000' }));
};
```
