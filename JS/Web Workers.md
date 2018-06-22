## 介绍
Web Worker为Web内容在后台线程中运行脚本提供了一种简单的方法。线程可以执行任务而不干扰用户界面。一旦创建， 一个worker 可以将消息发送到创建它的JavaScript代码, 通过将消息发布到该代码指定的事件处理程序 (反之亦然)

### 分类
一个worker是使用一个构造函数创建的一个对象(e.g. Worker()) 运行一个命名的JavaScript文件 - 这个文件包含将在工作线程中运行的代码; workers 运行在另一个全局上下文中,不同于当前的window. 因此，使用 window快捷方式获取当前全局的范围 (而不是self) 在一个 Worker 内将返回错误

#### 专用worker
在专用workers的情况下，[DedicatedWorkerGlobalScope](https://developer.mozilla.org/zh-CN/docs/Web/API/DedicatedWorkerGlobalScope) 对象代表了worker的上下文（专用workers是指标准worker仅在单一脚本中被使用），一个专用worker仅仅能被首次生成它的脚本使用

#### 共享worker
共享worker的上下文是[SharedWorkerGlobalScope](https://developer.mozilla.org/zh-CN/docs/Web/API/SharedWorkerGlobalScope)对象，共享worker可以同时被多个脚本使用

[Web Workers可以使用的函数和类](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Functions_and_classes_available_to_workers)


workers和主线程间的数据传递通过这样的消息机制进行——双方都使用postMessage()方法发送各自的消息，使用onmessage事件处理函数来响应消息（消息被包含在Message事件的data属性中）。这个过程中数据并不是被共享而是被复制。


## 专用worker
```js
/* main.js */

if (window.Worker) {
  // 生产一个专用worker（必须是同源的文件地址）
  const myWorker = new Worker('./worker.js')
  // 监听message事件
  myWorker.onmessage = (e) => {
    console.log('result：', e.data)
  }

  const num1 = Math.random() * 10
  const num2 = Math.random() * 10

  // 向worker传递数据
  myWorker.postMessage([num1, num2])
}
```

> 在worker内部，worker是有效的全局作用域
```js
/* worker.js */

// onmessage为worker上下文中message事件处理函数
onmessage = (e) => {
  const num1 = e.data[0]
  const num2 = e.data[1]

  const workerResult = num1 * num2
  // 发送消息到生成该worker的父文档
  postMessage(workerResult)
}
```

#### 终止worker

> 主线程
```js
/* main.js */

myWorker.terminate()
```

> worker线程
```js
/* worker.js */

close()
```

#### 处理错误
```js
/* main.js */

myWorker.onerror = (e) => {
  // 防止触发默认动作
  // e.preventDefault()

  console.log(e)
  // e.message: 可读性良好的错误消息
  // e.filename: 发生错误的脚本文件名
  // e.lineno: 发生错误时所在脚本文件的行号
}
```

#### 生成subworker
如果需要的话 worker 能够生成更多的 worker。这就是所谓的subworker，它们必须托管在同源的父页面内。而且，subworker 解析 URI 时会相对于父 worker 的地址而不是自身页面的地址。这使得 worker 更容易记录它们之间的依赖关系。

#### 引入脚本与库
Worker 线程能够访问一个全局函数`importScripts()`来引入脚本，该函数接受0个或者多个URI作为参数来引入资源

```js
importScripts();                        /* 什么都不引入 */
importScripts('foo.js');                /* 只引入 "foo.js" */
importScripts('foo.js', 'bar.js');      /* 引入两个脚本 */
```

**注意**：脚本的下载顺序不固定，但执行时会按照传入 `importScripts()` 中的文件名顺序进行。这个过程是同步完成的；直到所有脚本都下载并运行完毕， `importScripts()` 才会返回。

## 共享worker
一个共享worker可以被多个脚本使用——即使这些脚本正在被不同的window、iframe或者worker访问

**注意**：如果共享worker可以被多个浏览上下文调用，所有这些浏览上下文必须属于同源

```js
/* main.js */

if (window.SharedWorker) {
  // 生成一个共享worker
  const myWorker = new SharedWorker('./worker.js')

  // 监听message事件
  myWorker.port.onmessage = (e) => {
    console.log('result：', e.data)
  }

  const num1 = Math.random() * 10
  const num2 = Math.random() * 10

  // 向worker传递数据
  myWorker.port.postMessage([num1, num2])
}
```

```js
/* worker.js */

onconnect = function (e) {
    var port = e.ports[0];
 
    port.onmessage = function (e) {
        var workerResult = 'Result: ' + (e.data[0] * e.data[1]);
        port.postMessage(workerResult);
    }
}
```
个人理解：共享worker即一个在多个页面共享一个worker的执行上下文（包括变量存储）

**注意**：
1. 一个共享worker通信必须通过端口对象——一个确切的打开的端口供脚本与worker通信（在专用worker中这一部分是隐式进行的)
2. 在传递消息之前，端口连接必须被显式的打开，打开方式是使用`onmessage`事件处理函数(主进程或worker进程中)或者`start()`方法
3. 在使用`start()`方法打开端口连接时，如果父级线程和worker线程需要双向通信，那么它们都需要调用`start()`方法

- - -
- [使用 Web Workers-MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers)