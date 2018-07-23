## 浏览器缓存过程
1. 浏览器第一次加载资源，服务器返回`200`，浏览器将资源文件从服务器上请求下载下来，并把`Response Headers`及`该请求的返回时间`一并缓存；

2. 下一次加载资源时，先比较当前时间和上一次返回`200`时的`时间差`，如果没有超过`Cache-Control`设置的`max-age`，则没有过期，命中**强缓存**，不发请求直接从本地缓存读取该文件（如果浏览器不支持`HTTP1.1`，则用`Expires`判断是否过期）,返回`200`；如果时间过期，则向服务器发送header带有`If-None-Match`和`If-Modified-Since`的请求（分别对应响应头中的`Etag`和`Last-Modified`）；

3. 服务器收到请求后，优先根据`Etag`的值（对应请求头中的` If-None-Match`）判断被请求的文件有没有做修改，`Etag`值一致则没有修改，命中协商缓存，返回`304`；如果不一致则有改动，直接返回新的资源文件带上新的`Etag`值并返回`200`；

4. 如果服务器收到的请求没有`Etag`值（即请求头中`If-None-Match`的值），则将`If-Modified-Since`和被请求文件的最后修改时间（`Last-Modified`）做比对，一致则命中协商缓存，返回`304`；不一致则返回新的`Last-Modified`和文件并返回`200`；

5. 如果 `If-None-Match`和`If-Modified-Since`组合使用，需两者都通过才认为资源未修改

## 浏览器缓存类型
1. **强缓存**：
    不会向服务器发送请求，直接从缓存中读取资源（在chrome控制台的network选项中可以看到该请求返回`200`的状态码，并且size显示`from disk cache`或`from memory cache`）
2. **协商缓存**：
    向服务器发送请求，服务器会根据这个请求的request header的一些参数来判断是否命中协商缓存，如果命中，则返回`304`状态码并带上新的response header通知浏览器从缓存中读取资源

**共同点**：都是从客户端缓存中读取资源
**区别**：强缓存不会发请求，协商缓存会发请求

## 缓存有关的Headers
### 强缓存
####  响应头中的[`Cache-Control`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control) (http1.1)

- **`Cache-Control: max-age=315360000`**  
以秒为单位，未到期时不请求新数据，到期时效果等同于`no-cache`  

`Cache-Control`常用值：
```
public: 共有缓存，可被缓存代理服务器缓存,比如CDN
private: 私有缓存，不能被共有缓存代理服务器缓存，可被用户的代理缓存如浏览器。

max-age=[秒]：表示在这个时间范围内缓存是新鲜的无需更新。类似Expires时间，不过这个时间是相对的，而不是绝对的。也就是某次请求成功后多少秒内缓存是新鲜的。

s-maxage=[秒]：类似max-age, 除了仅应用于共享缓存（如代理）。

no-cache：每次在使用缓存之前都强制发送请求给源服务器进行验证，检查文件该没改变(其实这里和ETag/Last-Modified区别不大)

no-store：就是禁止缓存，不让浏览器保留缓存副本

must-revalidate：告诉浏览器，你这必须再次验证检查信息是否过期, 返回的代号就不是200而是304了。

proxy-revalidate：类似must-revalidate，除了只能应用于代理缓存。
```

多个指令以逗号分隔。

```
//Response Headers
Cache-Control:private, max-age=0, must-revalidate
```

该文件是一个私有文件,只能被浏览器缓存，而不能被代理缓存。`max-age`标识该缓存立即过期，其实和`no-cache`实际上区别不大. 然后`must-revalidate`告诉浏览器，你必须给我再验证文件过没过期，比如接下来可能会验证`Last-Modified`或者`ETag`。如果没有过期则使用本地缓存。

等同于:

```
//Response Headers
Cache-Control:private,no-cache
```


#### 请求头中的`Cache-Control`
- `Cache-Control: max-age=0`：表示缓存需要进行验证(`ETag` || `Last-Modified`)，如果缓存未过期,则可以使用
- `Cache-Control: no-cache`：表示浏览器只能获取最新的文件,和Response Header中的`no-store`相对应


#### Expires (http1.0)
**与`Cache-Control`同时出现时，以`Cache-Control`为准**

`Expires:Thu,01 Dec 2015 16:00:00 GMT`

### 协商缓存
判断条件(两组,可单独使用也可组合使用，组合使用时需两者都通过才认为资源未修改)：

- **ETag (响应头) 和 If-None-Match (请求头)**  
判断资源是否修改的一种方法，并不依赖资源的修改时间，而是根据资源内容计算出一个唯一的值，譬如计算文件的MD5值
- **Last-Modified (响应头)  和 If-Modified-Since (请求头)**  
标志该资源的最近修改时间  
**注意**：如果响应头中有 `Last-modified` 而没有 `Expire` 或 `Cache-Control` 时，浏览器会有自己的算法来推算出一个时间缓存该文件多久，不同浏览器得出的时间不一样，所以 `Last-modified` 要记得配合 `Expires/Cache-Control` 使用

**说明：** 浏览器在缓存资源的同时也会把这些头信息缓存起来，在请求发起的时候，如果之前缓存了`Last-Modified`，在发起请求的时候就会带一个`If-Modified-Since`的头，`If-Modified-Since`值为缓存的`Last-Modified的值`；如果返回的是`ETag`，则在发起请求的时候就会带一个`If-None-Match`的头，`If-None-Match`值为缓存的`ETag`的值。（服务器在接收到后也会做比对，如果相同则命中**协商缓存**，返回`304`，从缓存中读取资源）

**比较**：
- 在精确度上，`Etag`要优于`Last-Modified`。（`Last-Modified`的时间单位是秒，每次修改都会更新`Etag`）
- 在性能上，`Etag`要逊于`Last-Modified`，`Last-Modified`只需要记录时间，而Etag需要服务器通过算法来计算出一个hash值
- 优先级上，服务器校验优先考虑`Etag`

![http-cache](/.assets/images/http-cache.png)

## 更新资源的方式
- html一般采用`Cache-Control:no-cache,must-revalidate`
（css、js、图片资源放在CDN上，利用`Cache-Control:max-age=...`去做一个长cache）
- 在资源引用路径后加时间戳
- 利用前端自动化工具修改资源文件名
- 客户端刷新：（地址栏访问，链接跳转是正常用户行为，将会触发浏览器缓存机制；）
    - **F5(Normal Reload)**：浏览器会设置max-age=0，跳过强缓存判断，会进行协商缓存判断
    `Cache-Control:max-age=0`    
    `If-Modified-Since` or `If-None-Match`   
    - **Ctrl + F5 (Hard Reload)**：跳过强缓存和协商缓存，直接从服务器拉取资源
    `Cache-Control:no-cache`
    `Pragma:no-cache`


**Reference**：
- [浅谈浏览器http的缓存机制](http://www.cnblogs.com/vajoy/p/5341664.html)
- [浏览器的缓存机制小结](https://github.com/chenjiangsong/blog/issues/1)
- [浏览器缓存知识小结及应用](http://www.cnblogs.com/lyzg/p/5125934.html)
- [浏览器的缓存(1)](https://segmentfault.com/a/1190000004486640)
- [浏览器的缓存(2)](https://segmentfault.com/a/1190000004486660)
- [设计一个无懈可击的浏览器缓存方案：关于思路，细节，ServiceWorker，以及HTTP/2](https://zhuanlan.zhihu.com/p/28113197)
