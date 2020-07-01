# 网络层通信

- AJAX/Fetch/axios
- HTTP 1.0/2.0（双向通道的好处，什么时候用2.0，HTTPS）
- TCP（三次握手四次挥手）
- 缓存（DNS，强缓存 & 协商缓存）
- 跨域处理方案
- 性能优化

实战中，
- 登录注册的前后端处理机制
- 加密策略：encodeURI-Component，MD5等
- 存储方案：cookie，webStorage，session等
- 用户权限和登陆态的校验处理
- Token的校验处理

## Q：跨域的实现方案
跨域产生的原因：前后端分离
常见问题：你服务器是怎么部署的？（Linux + Nginx + Docker）

### JSONP
### iframe
window.name
document.domain
location.hash
post.message
### CORS跨域资源共享
客户端
登陆验证：第一次向服务器发请求，服务器告诉你成功了，会通过JWT算法生成一个TOKEN，把TOKEN直接给你，当客户端拿到TOKEN之后，可以存储到vuex/localstorage中。再一次发请求，我们需要在请求拦截器上把token都带上，`config.headers.Authorization = token`，即在请求头里带上Authorization字段。服务器再次接收到请求，再通过JWT算法进行校验，如果是之前给的token，说明是合法。这就保证了接口调用信息的安全性。

服务器端
Access-Control-Allow-Origin
### 基于http proxy实现跨域请求

### Nginx反向代理
