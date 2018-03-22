```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                    href                                     │
├──────────┬┬───────────┬─────────────────┬───────────────────────────┬───────┤
│ protocol ││   auth    │      host       │           path            │ hash  │
│          ││           ├──────────┬──────┼──────────┬────────────────┤       │
│          ││           │ hostname │ port │ pathname │     search     │       │
│          ││           │          │      │          ├─┬──────────────┤       │
│          ││           │          │      │          │ │    query     │       │
"  http:   // user:pass @ host.com : 8080   /p/a/t/h  ?  query=string   #hash "
│          ││           │          │      │          │ │              │       │
└──────────┴┴───────────┴──────────┴──────┴──────────┴─┴──────────────┴───────┘
```

## URI和URL
### URI (Uniform Resource Identifier)
统一资源标识符。由两个主要的子集构成：
- **URL**: (Uniform Resource Locator)统一资源定位符（URL正是使用web浏览器等访问web页面时需要输入的网页地址）
- **URN**：通过名字来识别资源的，与它们当前所处位置无关

#### URL
**语法**：  
```bash
<scheme>://<user>:<password>@<host>:<port>/<path>:<params>?<query>#<frag>
```

**方案(scheme)**：  
scheme大小写无关

**常见scheme**

##### 1. HTTP
HTTP是一种超文本传输协议方案，除了没有用户名和密码之外，与通用的URL格式相符。如果省略了端口，就默认为80

基本格式:
```bash
http://<host>:<port>/<path>?<query>#<frag>
```

##### 2. HTTPS
方案https与方案http是一对。唯一的区别在于方案https使用了网景的SSL， SSL为HTTP连接提供了端到端的加密机制。其语法与HTTP的语法相同，默认端口为443

基本格式：
```bash
https://<host>:<port>/<path>?<query>#<frag>
```

##### 3. Mailto
Mailto URL指向的是E-mail地址。由于E-mail的行为与其他方案都有所不同(它并不指向任何可以直接访问的对象)，所以mailto URL的格式与标准URL的格式也有所不同

基本格式
```bash
mailto:joe@hardware.com
```

##### 4. ftp
文件传输协议URL可以用来从FTP服务器上下载或向其上传文件，并获取FTP服务器上的目录结构内容的列表（在Web和URL出现之前，FTP就已经存在了。Web应用程序将FTP作为一种数据访问方案使用）

基本格式
```bash
ftp://<user>:<password>@<host>:<port>/<path>;<params>
```

##### 5. rtsp,rtspu
RTSP URL是可以通过实时流传输协议(Real Time Streaming Protocol)解析的音/视频媒体资源的标识符, rtspu中的u表示它是使用UDP协议来获取资源的

基本格式
```bash
rtsp://<user>:<password>@<host>:<port>/<path>
rtspu://<user>:<password>@<host>:<port>/<path>
```

##### 6. file
方案file表示一台指定主机(通过本地磁盘、网络文件系统或其他一些文件共享系统)上可直接访问的文件。各字段都遵循通用格式。如果省略了主机名，就默认为正在使用URL的本地主机。

基本格式
```bash
file://<host>/<path>
```

##### 7. telnet
方案telnet用于访问交互式业务。它表示的不是对象自身，而是可通过telnet协议访问的交互式应用程序(资源)

基本格式
```bash
telnet://<user>:<password>@<host>:<port>/
```


- - -
**参考**：
- [前端学HTTP之URL](https://www.cnblogs.com/xiaohuochai/p/6144157.html)