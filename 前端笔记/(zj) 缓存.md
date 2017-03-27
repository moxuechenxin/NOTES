## 1. 是否缓存主要有两个返回控制头
### 1.1 [Cache-Control](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control) (http1.1)
**与Expires同时出现时，以Cache-Control为准**
- **Cache-Control:max-age=315360000**  
以秒为单位，未到期时不请求新数据，到期时效果等同于no-cache  
- **Cache-Control:no-cache**  
这个网络资源， 可以在cache里面放一份，使用前先询问服务器该缓存资源是否最新，是则使用该缓存资源  
- **Cache-Control:no-store**  
这个网络资源不能缓存，每次都重新请求拉取  

### 1.2 Expires (http1.0)
Expires:Thu,01 Dec 2015 16:00:00 GMT

## 2. 请求头中的Cache-Control
- **Cache-Control: max-age=0**  
\- 表示缓存需要进行验证(ETag||Last-Modified)，如果缓存未过期,则可以使用
- **Cache-Control: no-cache**  
\- 表示浏览器只能获取最新的文件,和Response Header中的no-store相对应

## 3. 304 Not Modified
判断条件(两组,可单独使用也可组合使用，组合使用时需两者都通过才认为资源未修改)：
- **ETag (响应头) 和 If-None-Match (请求头)**  
\- 判断资源是否修改的一种方法，并不依赖资源的修改时间，而是根据资源内容计算出一个唯一的值，譬如计算文件的MD5值
- **Last-Modified (响应头)  和 If-Modified-Since (请求头)**  
\- Last-Modified：标志该资源的最近修改时间  
**说明：** 浏览器在缓存资源的同时也会把这些头信息缓存起来，在请求发起的时候，如果之前缓存了Last-Modified，在发起请求的时候就会带一个If-Modified-Since的头，If-Modified-Since值为缓存的Last-Modified的值；如果返回的是ETag，则在发起请求的时候就会带一个If-None-Match的头，If-None-Match值为缓存的ETag的值

## 4. 更新资源的方式
### 4.1 html一般采用Cache-Control:no-cache,must-revalidate
（css、js、图片资源放在CDN上，利用Cache-Control:max-age=...去做一个长cache）
### 4.2 在资源引用路径后加时间戳
### 4.3 利用前端自动化工具修改资源文件名
### 4.4 客户端刷新
**F5(Normal Reload)**   
Cache-Control:max-age=0    
If-Modified-Since or If-None-Match   
**Ctrl + F5 (Hard Reload)**  
Cache-Control:no-cache
Pragma:no-cache

## 5. H5应用缓存 (AppCache)
### 5.1 优势：
- **离线浏览**
- **速度**
- **减少服务器负载**

### 5.2 兼容性：
除了IE9及以下，所有主流浏览器都支持

### 5.3 manifest 文件可分为三个部分：
- **CACHE MANIFEST**  
 \- 在此标题下列出的文件将在首次下载后进行缓存，等价于CACHE
- **NETWORK**  
 \- 在此标题下列出的文件需要与服务器的连接，且不会被缓存
- **FALLBACK**  
 \- 在此标题下列出的文件规定当页面无法访问时的回退页面

### 5.4 使用AppCache方案的步骤：
- 整理出需要缓存的静态文件列表，如 jquery.js 和 gb.css。
- 配置服务器支持。
- 确定内容更新机制和浏览器兼容方案
### 用法：

```
<!--在demo.appcache文件中声明如何使用缓存-->
<!DOCTYPE html>
<html manifest="demo.appcache">
<head>
<meta charset="UTF-8">
<title></title>
</head>
<body>

</body>
</html>
```

## 6. HTTP Cache备选方案
### 6.1 LocalStorage
### 6.2 Web databases


**Reference**:  
- [浏览器的缓存(1)](https://segmentfault.com/a/1190000004486640)
- [浏览器的缓存(2)](https://segmentfault.com/a/1190000004486660)
- http://www.imooc.com/video/10627    
- https://github.com/fouber/blog/issues/6  
- https://aotu.io/notes/2016/03/16/optimization/  
- https://aotu.io/notes/2016/09/22/http-caching/  
- https://segmentfault.com/a/1190000004486640  
- http://div.io/topic/745
