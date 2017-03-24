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

当遇到网络错误时，`fetch()` 返回的 `promise` 会被 `reject`，并传回 `TypeError`，虽然这也可能因为权限或其它问题导致。成功的 `fetch()` 检查不仅要包括` promise` 被 `resolve`，还要包括 `Response.ok` 属性为 `true`。`HTTP 404` 状态并不被认为是网络错误

## request 和 response共有
### headers对象
通过构造函数`Headers`可以返回一个`headers对象`。一个 `headers` 对象是一个简单的多名值对

```js
var content = "Hello World";
var myHeaders = new Headers();
myHeaders.append("Content-Type", "text/plain");
myHeaders.append("Content-Length", content.length.toString());
myHeaders.append("X-Custom-Header", "ProcessThisImmediately");

// 也可以传一个多维数组或者对象字面量
myHeaders = new Headers({
    "Content-Type": "text/plain",
    "Content-Length": content.length.toString(),
    "X-Custom-Header": "ProcessThisImmediately",
});
```

其内容可以被获取：

```js
console.log(myHeaders.has("Content-Type")); // true
console.log(myHeaders.has("Set-Cookie")); // false
myHeaders.set("Content-Type", "text/html");
myHeaders.append("X-Custom-Header", "AnotherValue");
 
console.log(myHeaders.get("Content-Length")); // 11
console.log(myHeaders.getAll("X-Custom-Header")); // ["ProcessThisImmediately", "AnotherValue"]
 
myHeaders.delete("X-Custom-Header");
console.log(myHeaders.getAll("X-Custom-Header")); // [ ]
```

**注意**：
- 如果使用了一个不合法的HTTP Header属性名，那么Headers的方法通常都抛出 TypeError 异常。
- 如果不小心写入了一个不可写的属性，也会抛出一个 TypeError 异常。除此以外的情况，失败了并不抛出异常

```js
var myResponse = Response.error();
try {
    myResponse.headers.set("Origin", "http://mybank.com");
} catch(e) {
    console.log("Cannot pretend to be a bank!");
}
```

检查 content type 是否正确

```js
fetch(myRequest).then(function(response) {
    if(response.headers.get("content-type") === "application/json") {
        return response.json().then(function(json) {
          // process your JSON further
        });
  } else {
    console.log("Oops, we haven't got JSON!");
  }
});
```
#### Headers 对象的`Guard`属性
**可选值**：
- `none`：默认的
- `request`：从 request 中获得的 headers（Request.headers）只读
- `request-no-cors`：从不同域（Request.mode no-cors）的 request 中获得的 headers 只读
- `response`：从 response 中获得的 headers（Response.headers）只读
- `immutable`：在 ServiceWorkers 中最常用的，所有的 headers 都只读

**注意**：
不可以添加或者修改一个 `guard` 属性是 `request` 的 `Request Headers` 的 `Content-Length` 属性。同样地，插入 `Set-Cookie` 属性到一个 `response headers` 是不允许的，因此 `ServiceWorkers` 是不能给合成的 `Response` 的 `headers` 添加一些 `cookies`

### body对象
可以是以下类型：
- **ArrayBuffer**
- **ArrayBufferView (Uint8Array and friends)**
- **Blob/File**
- **String**
- **URLSearchParams**
- **FormData**

`Body 类`定义了以下方法 (这些方法都被 `Request` 和`Response`所实现)以获取body内容. 这些方法都会返回一个被解析后的`promise`对象和数据：
- **arrayBuffer()**
- **blob()**
- **json()**
- **text()**
- **formData()**

请求体可以由传入body参数来进行设置:

```js
var form = new FormData(document.getElementById('login-form'));

fetch("/login", {
    method: "POST",
    body: form
})
```
request 和response (也包括fetch() 方法)都会试着自动设置content type.如果没有设置Content-Type值，发送的请求也会自动设值

## request
`fetch(target, options)`

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

### Request 构造函数
传递的参数同`fetch()`。返回一个`Request对象`

```js
var myHeaders = new Headers();

var myInit = { 
    method: 'GET',
    headers: myHeaders,
    mode: 'cors',
    cache: 'default' 
};

var myRequest = new Request('flowers.jpg',myInit);

fetch(myRequest,myInit)
.then(function(response) {
     return response.blob();
})
.then(function(myBlob) {
     var objectURL = URL.createObjectURL(myBlob);
     myImage.src = objectURL;
});
```

可以传入一个已存在的 `request 对象`来创造一个拷贝

```js
var anotherRequest = new Request(myRequest,myInit);
```
`request` 和 `response bodies` 只能被使用一次, 创建一个拷贝就可以再次使用 `request/response` 了，可以使用不同的 `init 参数`


## Response 对象
获取`Responde对象`的方式：
- 在`fetch()`处理完promises之后返回`Response对象`
- 通过`Response构造函数`（只有在`ServiceWorkers`中才真正有用,当使用`respondWith()`方法并提供了一个自定义的`response`来接受`request`时）:

```js
var myBody = new Blob();

addEventListener('fetch', function(event) {
    event.respondWith(new Response(myBody, {
        headers: { 
            "Content-Type" : "text/plain" 
        }
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
- [【翻译】这个API很“迷人”——(新的Fetch API)](https://www.w3ctech.com/topic/854)