# http-proxy-middleware
https://github.com/chimurai/http-proxy-middleware

- `proxy('**', {...})` matches any path, all requests will be proxied.
- `proxy('**/*.html', {...})` matches any path which ends with `.html`
- `proxy('/*.html', {...})` matches paths directly under path-absolute
- `proxy('/api/**/*.html', {...})` matches requests ending with `.html` in the path of `/api`
- `proxy(['/api/**', '/ajax/**'], {...})` combine multiple patterns
- `proxy(['/api/**', '!**/bad.json'], {...})` exclusion
