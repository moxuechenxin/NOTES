## 介绍
Fetch API  提供了一个 JavaScript接口，用于访问和操纵HTTP管道的部分，例如请求和响应。它还提供了一个全局 fetch()方法，该方法提供了一种简单，合乎逻辑的方式来跨网络异步获取资源。

**与 XMLHttpRequest 区别**：
- 从 `fetch()`返回的 `Promise` 将不会拒绝HTTP错误状态, 即使响应是一个 HTTP `404` 或 `500`。相反，它会正常解决 (其中ok状态设置为false),  并且仅在网络故障时或任何阻止请求完成时，它才会拒绝。
- 默认, `fetch` 将不会发送或接收任何 `cookies` 从服务器, 如果站点依赖于维护一个用户会话，则导致未经认证的请求(要发送 cookies，必须发送凭据头)

**参考**：
- [使用 Fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)
- [深入浅出Fetch API](http://wwsun.github.io/posts/fetch-api-intro.html)