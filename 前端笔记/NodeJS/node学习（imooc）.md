## 简易web服务器

```js
const http = require('http')
http.createServer((req, res) => {
  res.writeHead(200, {
    'Content-Type': 'text/plain'
  })
  res.end('Hello World \n')
}).listen(5555, '127.0.0.1')
console.log('Server running at http://127.0.0.1:5555')
```

## URL

```js
const url = require('url')
url.parse('http://imooc.com:8080/course/list?from=scott&course=node#floor1')

// 输出
/*
{
  protocol: 'http:',
  slashes: true,
  auth: null,
  host: 'imooc.com:8080',
  port: '8080',
  hostname: 'imooc.com',
  hash: '#floor1',
  search: '?from=scott&course=node',
  query: 'from=scott&course=node',
  pathname: '/course/list',
  path: '/course/list?from=scott&course=node',
  href: 'http://imooc.com:8080/course/list?from=scott&course=node#floor1'
}
*/
```
## querystring

```js
const querystring = require('querystring')
querystring.stringify({ foo: 'bar', baz: ['qux', 'quux'], corge: '' })
// 返回 'foo=bar&baz=qux&baz=quux&corge='

querystring.stringify({foo: 'bar', baz: 'qux'}, ';', ':')
// 返回 'foo:bar;baz:qux'

querystring.parse('foo=bar&abc=xyz&abc=123')
// 返回
/*
{
  foo: 'bar',
  abc: ['xyz', '123']
}
*/
```
## http
### 浏览器输入网址，回车后发生了什么：
1. chrome搜索自身的DNS缓存（chrome://net-internals/#dns）
2. 搜索操作系统自身的DNS缓存（浏览器没有找到缓存或缓存已经失效）
3. （系统DNS缓存没有）读取本地的HOST文件
4. 浏览器发起一个DNS的一个系统调用
    1. 宽带运营商服务器查看本身缓存
    2. （没有找到缓存）运营商服务器发起一个迭代DNS解析的请求，运营商服务器把结果返回操作系统内核同时缓存起来，然后操作系统内核把结果返回给浏览器
5. 浏览器获得域名对应的IP地址后，发起HTTP "三次握手"
6. TCP/IP链接建立后，浏览器就可以向服务器发送HTTP请求了，使用了比如说，用HTTP的GET方法请求一个根域里的一个域名，协议可以采用HTTP 1.0的一个协议
7. 服务器端接受到了这个请求，根据路径参数，经过后端的一些处理之后，把处理后的一个结果的数据返回给浏览器，如果是慕课网的页面就会把完整的HTML页面代码返回给浏览器
8. 浏览器拿到了慕课网的完整的HTML页面代码，在解析和渲染这个页面的时候，里面的JS、CSS、图片静态资源，他们同样也是一个个HTTP请求都需要经过上面的主要的7个步骤

### 什么是回调？
### 什么是同步/异步？
### 什么是I/O？
### 什么是单线程/多线程？
### 什么是阻塞/非阻塞？
### 什么是事件？
### 什么是事件驱动？
### 什么是基于事件驱动的回调？
### 什么是事件循环？
### 什么是作用域？
### 什么是上下文？
this

## http性能

## 爬虫

```js
/* node内置模块 */
let http = require('http')
let https = require('https')
let fs = require('fs')
let path = require('path')
let url = require('url')

/* 第三方模块 */
let cheerio = require('cheerio')

// 网址
let reqUrl = 'http://reactjs.cn/react/docs/getting-started-zh-CN.html'
// 网页根路径
let rootDirName = 'react_doc'

/**
 * 先循环生成目录，再写入文件
 * @param  {String}   savePath  [文件保存路径]
 * @param  {String}   data      [文件内容]
 * @return {[type]}             [description]
 */
const writeFile = (savePath, data) => {
  if (!fs.existsSync(savePath)) {
    let dir = path.parse(savePath).dir
    let dirArr = dir.split('/')
    let currDir = ''
    dirArr.forEach(item => {
      currDir += item + '/'
      try {
        fs.mkdirSync(currDir)
      } catch (e) {
      }
    })
    fs.writeFileSync(path.resolve(savePath), data)
  }
}

/**
 * 获取并保存资源
 * @param  {String}   reqUrl    [线上资源地址]
 * @param  {String}   savePath  [文件保存路径]
 * @param  {Function} callback  [回调]
 */
const saveResource = (reqUrl, savePath, callback) => {
  if (reqUrl.indexOf('//') === -1) {
    reqUrl = 'http://reactjs.cn' + reqUrl
  } else if (reqUrl.indexOf('//') === 0) {
    reqUrl = 'http:' + reqUrl
  }
  let urlObj = url.parse(reqUrl);
  let urlPath = urlObj.path;
  (urlObj.protocol.slice(0,-1) === 'http' ? http : https).get(reqUrl, (res) => {
    // 响应头
    let contentType = res.headers['content-type']
    let resData = ''
    res.on('data', (chunk) => {
      resData += chunk
    })
    res.on('end', () => {
      // 文件应该存放的相对位置
      savePath = savePath || path.join(rootDirName, urlPath)
      if (contentType === 'text/html') {
        let $ = cheerio.load(resData)
        // css外链
        let styleUrls = Array.from($('link[rel="stylesheet"]').map((index, item) => {
          return $(item).attr('href')
        })).sort((a, b) => {
          return a.indexOf('//') === -1 ? -1 : 1
        })
        // js外链
        let scriptUrls = Array.from($('script[src]').map((index, item) => {
          return $(item).attr('src')
        })).sort((a, b) => {
          return a.indexOf('//') === -1 ? -1 : 1
        })
        let assetsUrl = ''
        styleUrls.concat(scriptUrls).forEach((item, index) => {
          let localPath = ''
          if (item.indexOf('//') === -1) {
            assetsUrl = path.parse(item).dir
          } else {
            // 替换公网外链为路径外链
            let fileName = path.parse(url.parse(item, true, true).path).base
            localPath = path.join(rootDirName, assetsUrl, fileName)
            // 替换html中的资源地址
            resData = resData.replace(new RegExp(item, 'g'), localPath)
          }
          saveResource(item, localPath)
        })

      }
      writeFile(savePath, resData)
    })
  })
}

saveResource(reqUrl)
```
## 事件模块（events）

```js
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {};

const myEmitter = new MyEmitter();

// 注册监听器
myEmitter.on('event', (data) => {
  console.log('发生了一个事件！');
  console.log('传递的参数！', data);
});
// 触发事件
myEmitter.emit('event', '参数');
```

## HTTP：get/request

```js
var postData = querystring.stringify({
  'msg' : 'Hello World!'
});

var options = {
  hostname: 'www.google.com',
  port: 80,
  path: '/upload',
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',
    'Content-Length': Buffer.byteLength(postData)
  }
};

var req = http.request(options, (res) => {
  console.log(`STATUS: ${res.statusCode}`);
  console.log(`HEADERS: ${JSON.stringify(res.headers)}`);
  res.setEncoding('utf8');
  res.on('data', (chunk) => {
    console.log(`主体: ${chunk}`);
  });
  res.on('end', () => {
    console.log('响应中已无数据。');
  });
});

req.on('error', (e) => {
  console.log(`请求遇到问题: ${e.message}`);
});

// 写入数据到请求主体
req.write(postData);
req.end();
```
