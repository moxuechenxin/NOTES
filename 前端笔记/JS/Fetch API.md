## 介绍
`Fetch API` 提供了一个 JavaScript接口，用于访问和操纵HTTP管道的部分，例如请求和响应。它还提供了一个全局`fetch()`方法，该方法提供了一种简单，合乎逻辑的方式来跨网络异步获取资源。`fetch()`返回的是一个`Promise`对象，让你能够对请求的返回结果进行检索

**与 XMLHttpRequest 区别**：
- 从 `fetch()`返回的 `Promise` 将不会拒绝HTTP错误状态, 即使响应是一个 HTTP `404` 或 `500`。相反，它会正常解决 (其中ok状态设置为false),  并且仅在网络故障时或任何阻止请求完成时，它才会拒绝。
- 默认, `fetch` 将不会发送或接收任何 `cookies` 从服务器, 如果站点依赖于维护一个用户会话，则导致未经认证的请求(要发送 cookies，必须发送凭据头)

```js
// 通过网络获取一个图像并将其插入到一个 <img> 元素中
let myImage = document.querySelector('img');

fetch('flowers.jpg')
.then(function(response) {
    return response.blob();
})
.then(function(myBlob) {
    let objectURL = URL.createObjectURL(myBlob);
    myImage.src = objectURL;
});
```
**语法**：

```js
fetch(url, opts)
```

**参考**：
- [使用 Fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)
- [深入浅出Fetch API](http://wwsun.github.io/posts/fetch-api-intro.html)