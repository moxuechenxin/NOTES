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
### 语法：

```js
fetch(target, options).then(function(response) {
  // handle HTTP response
}, function(error) {
  // handle network error
})
```

### 返回值：
`fetch(target, options)`返回一个 `Promise`，`resolve` 时回传 `Response对象`

## request

### target
定义要获取的资源, 可选值：
- 一个 `USVString` 字符串，包含要获取资源的 `URL`。
- 一个 `Request对象`
    
### options参数：
- **method**：**默认值为`GET`**。请求使用的方法，如 `GET`、`POST`
- **headers**：**默认值为`{}`**。请求的头信息，形式为 `Headers对象`或 `ByteString`
- **body**: 请求的 body 信息：可能是一个 `Blob`、`BufferSource`、`FormData`、`URLSearchParams` 或者 `USVString` 对象。**注意 GET 或 HEAD 方法的请求不能包含 body 信息**
- **mode**：请求的模式，如 `cors`、 `no-cors` 或者 `same-origin`
- **cache**：请求的 `cache` 模式: `default`, `no-store`, `reload`, `no-cache`, `force-cache`, or `only-if-cached`
- **credentials**: 请求的 `credentials`，如 `omit`、`same-origin` 或者 `include`
- **redirect**: 可用的 `redirec`t 模式: `follow` (自动重定向), `error` (如果产生重定向将自动终止并且抛出一个错误), 或者` manual` (手动处理重定向). 在Chrome中，Chrome 47之前的默认值是 follow，从 Chrome 47开始是 manual
- **referrer**：**默认值为`client`**。一个 `USVString`, 可以是 `no-referrer`、`client`或一个 `URL`
- **referrerPolicy**
- **integrity**

## Response 对象
获取`Responde对象`的方式：
- 在`fetch()`处理完promises之后返回`Response对象`
- 通过`Response构造函数`（只有在`ServiceWorkers`中才真正有用,当使用`respondWith()`方法并提供了一个自定义的`response`来接受`request`时）:

```js
var myBody = new Blob();

addEventListener('fetch', function(event) {
  event.respondWith(new Response(myBody, {
    headers: { "Content-Type" : "text/plain" }
  });
});
```
### Response构造函数
#### 参数：
1. response的数据体
2. 一个初始化对象(与`Request()`所接受的`init`参数类似)

### Response对象属性
- **status**：[Number]。**默认值为200**。为response的状态码
- **statusText**：[String]。**默认值为`"OK"`**,该值与HTTP状态码消息对应
- **ok**：[Boolean]。检查response的状态是否在`200-299`(包括200,299)这个范围内

**参考**：
- [使用 Fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)
- [fetch()](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalFetch/fetch)
- [深入浅出Fetch API](http://wwsun.github.io/posts/fetch-api-intro.html)
- [Fetch API (polyfill)](https://github.com/github/fetch)