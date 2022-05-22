#### CSRF攻击
 - CSRF攻击实际上不是盗用cookie
 - 而是在用户登陆过A网站的时候又访问了网站B，由于浏览器已经保存了cookieA，所以B网站可以向A网站发送一个简单请求（GET,POST,HEAD）
 - 简单请求的情况下，浏览器不会发出预验证请求（OPTION），而是直接向A网站发送一个携带了cookieA的请求
 - 跨域请求并非浏览器阻止请求发送，而是浏览器阻止js使用该请求的返回值，这代表着这个请求还是实际发送到了网站A
 - 如果A网站只校验cookieA，并执行了业务逻辑，那么就已经达成了CSRF攻击的目的
 - 注意，由于同源策略，所以无论在任何情况下，网站B并不能实际访问到cookieA


#### 防御CSRF攻击
- 后端校验refer，只接受来自A网站的请求
- cookie设置same-site=strict，只能在同一个站点下使用
- cookie设置same-site=lax，可以在同一个站点下使用，在不同站点下只能发送get或者资源预加载请求
- CSRF-TOKEN 由上可知，其实可以放进cookie里，因为网站B并没有真正的拿到cookieA，只是网站A的server端绝不能从cookie里校验CSRF-TOKEN，而是应该让前端将CSRF-TOKEN塞进body或者form里或者header里，这是网站B无法伪造的数据
- 即手动携带该token进行校验