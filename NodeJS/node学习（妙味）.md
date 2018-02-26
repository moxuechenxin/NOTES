## tips
- node的全局对象为`global`

## 模块
描述：一个文件就是一个模块，每个模块都有自己的作用域

### 模块内的变量
- `__dirname`：当前目录被解析过后的绝对路径
- `__filename`：当前文件被解析过后的绝对路径

### 模块加载

```js
// a.js

console.log('from a.js')
```
```js
// b.js

// 注意：当前路径用'./'
// 查找规则：
// 1. 首先按照加载的模块文件名称进行查找
// 2. 如果没有找到，加上后缀`.js`再进行查找
// 3. 如果没有找到，加上后缀`.json`再进行查找
// 4. 如果没有找到，加上后缀`.node`再进行查找
require('./a.js')

```
### 变量导入和导出
1. 把变量作为`global`对象的属性（不推荐）
2. 使用模块对象`module`

#### module对象
保存提供和当前模块有关的一些属性。在`module`对象中有一个子对象`exports`对象，可以通过这个对象把一个模块中的局部变量导出。通过`require(url)`导入模块的`exports`对象

## global对象
### __dirname（模块内部）
当前模块的目录名（绝对路径）。 等同于 `__filename` 的 `path.dirname()`。（`__dirname` 实际上不是一个全局变量，而是每个模块内部的）

### __filename（模块内部）
当前模块的文件名。（这是当前模块文件的解析后的绝对路径）

### process对象
node进程对象
#### process.argv
返回一个包含命令行参数的数组。第一个项是node程序，第二个项是js文件，然后是命令行传入的参数

#### process.exacPath
返回开启当前进程的绝对路径（node程序的绝对路径）

#### process.env
返回用户环境信息

#### process.stdin
标准输入流

```
// 监听用户的输入数据
process.stdin.on('data', function(chunk) {
  console.log('用户输入了:', chunk.toString())
})
```

#### process.stdout
标准输出流

- **process.stdout.write(message)**

## Buffer
用于操作二进制数据流

### Buffer.from()
#### Buffer.from(array)
返回一个新建的包含所提供的字节数组的副本的 Buffer。

#### Buffer.from(arrayBuffer[, byteOffset [, length]])
返回一个新建的与给定的 ArrayBuffer 共享同一内存的 Buffer。

#### Buffer.from(buffer)
返回一个新建的包含所提供的 Buffer 的内容的副本的 Buffer。

#### Buffer.from(string[, encoding])
返回一个新建的包含所提供的字符串的副本的 Buffer。

### Buffer.alloc()

#### Buffer.alloc(size[, fill[, encoding]])
返回一个指定大小的被填满的 Buffer 实例。 这个方法会明显地比 `Buffer.allocUnsafe(size)` 慢，但可确保新创建的 Buffer 实例绝不会包含旧的和潜在的敏感数据。

### Buffer.allocUnsafe()
#### Buffer.allocUnsafe(size) 与 Buffer.allocUnsafeSlow(size)
返回一个新建的指定 size 的 Buffer，但它的内容必须被初始化，可以使用`buf.fill(0)` 或完全写满。

### Buffer与字符编码
Node.js 目前支持的字符编码包括:
- **ascii** ： 仅支持 7 位 ASCII 数据。如果设置去掉高位的话，这种编码是非常快的。

- **utf8** ： 多字节编码的 Unicode 字符。许多网页和其他文档格式都使用 UTF-8 。

- **utf16le** ： 2 或 4 个字节，小字节序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）。

- **ucs2** ： 'utf16le' 的别名。

- **base64** ：Base64 编码。当从字符串创建 Buffer 时，按照 RFC4648 第 5 章的规定，这种编码也将正确地接受“URL 与文件名安全字母表”。

- **latin1** ：一种把 Buffer 编码成一字节编码的字符串的方式（由 IANA 定义在 RFC1345 第 63 页，用作 Latin-1 补充块与 C0/C1 控制码）。

- **binary** ： 'latin1' 的别名。

- **hex** ： 将每个字节编码为两个十六进制字符

## fs
 通过 `require('fs')` 使用该模块。 所有的方法都有异步和同步的形式

### fs.access(path[, mode], (err) => {})
测试 path 指定的文件或目录的用户权限

### fs.stat(path, (err, stats) => {})
返回文件信息

### fs.open(path, flags[, mode], callback)
异步地打开文件

**参数**：
- **path**：文件路径
- **flags**：操作方式：`r`（读取模式）、`r+`（读写模式）、`rs+`（同步读写模式）
- **mode**：文件模式
- **callback**: `function(err, fd) {}`

### fs.writeFile(filename, data [, options], callback)
异步地写入数据到文件，如果文件已经存在，则替代文件

### fs.appendFile(filename, data [, options], callback)
异步地追加数据到一个文件，如果文件不存在则创建文件

### fs.unlink(path, callback)
删除文件

### fs.rename(oldPath, newPath, (err) => {})
文件重命名

### fs.watch(filename[, options][, listener])
监视 filename 的变化，filename 可以是一个文件或一个目录。 返回的对象是一个 fs.FSWatcher。

### fs.mkdir(path[, mode], callback)
创建文件夹

### fs.rmdir(path, callback)
删除文件夹

### fs.readdir(path[, options], (err, files) => {})
读取一个目录的内容

## 前端项目自动化

```js
let fs = require('fs')
let path = require('path')

let dir = './miaov/js'
fs.watch(dir, (eventType, filename) => {
  fs.readdir(dir, (err, list) => {
    let arr = []
    list.forEach((item, index) => {
      arr.push(fs.readFileSync(path.join(dir, item), 'utf-8'))
    })
    console.log(arr.join(''))
    fs.writeFileSync(path.join(dir, '../_js/index.js'), arr.join(''))
  })
})
```

## http
通过 `require('http')` 使用该模块。

### http.createServer([requestListener])
返回一个新建的 `http.Server` 实例，
`requestListener` 是一个会被自动添加到 `request` 事件中的函数

### server.listen([port] [, hostname] [, backlog] [, callback])
`callback` 会被添加到 `listening` 事件的监听器中

```js
let http = require('http')
// 创建一个web服务器对象
let server = http.createServer()

server.listen()
```

### listening事件
当`server`调用`listen`方法并成功开始监听以后触发的事件

```js
let http = require('http')
let server = http.createServer()
server.on('listening', () => {
  console.log('listening...')
})
server.listen(80, 'localhost')
```

### error事件
当服务开启失败的时候触发`error`事件

```js
let http = require('http')
let server = http.createServer()
server.on('error', (err) => {
  console.log(err)
  server.listen(8999, 'localhost')
})
server.listen(80, 'localhost')
```

### request事件
当有客户端发送发送请求到该主机和端口的时候触发`request`事件

```js
let http = require('http')
let server = http.createServer()
server.on('request', (req, res) => {
  res.writeHead(200, {
      'content-type': 'text/plain; charset=utf-8'
  })
  res.end('后台返回的数据')
})
server.listen(8888, 'localhost')
```

### clientError 事件
`socket` 是发生错误的 `net.Socket` 对象

```js
const http = require('http');

const server = http.createServer((req, res) => {
  res.end();
});
server.on('clientError', (err, socket) => {
  socket.end('HTTP/1.1 400 Bad Request\r\n\r\n');
});
server.listen(8000);
```
当 `clientError` 事件发生是，不会有 `request` 或 `response` 对象，所以发送的任何 HTTP 响应，包括响应头和内容，必须被直接写入到 `socket` 对象。 必须小心确保响应是一个被正确格式化的`HTTP` 响应消息

### request对象
`http.IncomingMessage`的实例
**属性**：
- httpVersion：使用的http协议的版本
- headers：请求头信息中的数据
- url：请求的地址
- method：请求方式

### response对象
**方法**:
- **response.writeHead(statusCode[, statusMessage][, headers])**
该方法在消息中只能被调用一次，且必须在 response.end() 被调用之前调用

`headers`参数相当于调用`response.setHeader(key, value)`

```js
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, 'ok', {
    'content-type': 'text/html; charset=utf-8'
  })
  res.write('<h1>hello</h1>')
  res.end()
});

```

- **response.write(chunk[, encoding][, callback])**
如果该方法被调用且 `response.writeHead()` 还未被调用，则它会切换到隐式消息头模式并刷新隐式消息头。
它会发送一块响应主体。该方法可被多次调用，以便提供主体连续的部分。

- **response.end([data][, encoding][, callback])**
该方法告诉服务器所有响应头和主体都已被发送；服务器应将消息视为已完成。 对于每个响应，`response.end()` 方法必须被调用
`data`参数相当于调用`response.write()`

## url处理
url 模块提供了一些实用函数，用于 URL 处理与解析。通过 `require('url')` 使用该模块

```
let http = require('http')
let url = require('url')

let server = http.createServer()

server.on('request', (req, res) => {
  let path = url.parse(req.url).pathname
  res.writeHead(200, {
    'content-type': 'text/html;charset=utf-8'
  })
  switch (path) {
    case '/':
      res.writeHead(200, {
        'content-type': 'text/html;charset=utf-8'
      })
      res.end('<h1>首页</h1>')
      break
    case '/user':
      res.writeHead(200, {
        'content-type': 'text/html;charset=utf-8'
      })
      res.end('<h1>个人中心</h1>')
      break
    default:
      res.writeHead(404, {
        'content-type': 'text/html;charset=utf-8'
      })
      res.end('<h1>404</h1>')
      break
  }
})
server.listen(8888, 'localhost')

```

## 使用fs模块实现行为表现分离

```js
let http = require('http')
let url = require('url')
let fs = require('fs')
let path = require('path')

let server = http.createServer()

let htmlDir = path.join(__dirname, 'html')
function sendData (file, req, res) {
  fs.readFile(file, (err, data) => {
    if (err) {
      res.writeHead(404, {
        'content-type': 'text/html;charset=utf-8'
      })
      res.end('404')
    } else {
      res.writeHead(200, {
        'content-type': 'text/html;charset=utf-8'
      })
      res.end(data)
    }
  })
}

server.on('request', (req, res) => {
  let urlPath = url.parse(req.url).pathname
  urlPath = urlPath === '/' ? 'index.html' : ''
  res.writeHead(200, {
    'content-type': 'text/html;charset=utf-8'
  })
  console.log(urlPath, path.join('./html', urlPath))
  sendData (path.join('./html', urlPath), req, res)
})
server.listen(8888, 'localhost')
```

## querystring
querystring 模块提供了一些实用工具，用于解析与格式化 URL 查询字符串。通过`require('querystring')`

### querystring.parse(str[, sep[, eq[, options]]])

```js
querystring.parse('foo=bar&abc=xyz&abc=123')
/* 返回
{
  foo: 'bar',
  abc: ['xyz', '123']
}
*/
```

### querystring.stringify(obj[, sep[, eq[, options]]])

```
querystring.stringify({
    foo: 'bar',
    baz: ['qux', 'quux'],
    corge: ''
})
// 返回 'foo=bar&baz=qux&baz=quux&corge='
```

### post请求的数据处理
post发送的数据会被写入缓冲区中，需要通过`request`的`data`事件和`end`事件来进行数据拼接处理

```js
let http = require('http')
let url = require('url')
let server = http.createServer()

server.on('request', (req, res) => {
  if (req.method.toUpperCase() === 'POST') {
      let str = ''
      req.on('data', (chunk) => {
          str += chunk
      })
      req.on('end', () => {
          console.log(str)
      })
  }
})
server.listen(8888, 'localhost')
```