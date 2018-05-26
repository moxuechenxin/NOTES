## 属性
meta元素共有一个必选属性（content）和三个可选属性（http-equiv、name和scheme），content定义与 http-equiv 或 name 属性相关的元信息

### 必选属性
属性 |	值 |  描述
---|---|---
content	| some_text | 定义与 http-equiv 或 name 属性相关的元信息

### 可选属性
属性 |	值 |  描述
---|---|---
http-equiv | content-type / X-UA-Compatible / expire / refresh / set-cookie	|把content属性关联到HTTP头部
name |	author / description / keywords / generator / revised / others |	把 content 属性关联到一个名称
scheme | some_text | 定义用于翻译 content 属性值的格式

### name属性

```html
<!-- 页面作者 -->
<meta name="author" content="author name" />

<!-- 页面描述 -->
<meta name="description" content="meta元素共有三个可选属性(不超过150字符)" />

<!-- 页面关键词 -->
<meta name="keywords" content="meta标签总结,meta标签" />

<!-- 360 使用webkit内核渲染 -->
<meta name="renderer" content="webkit">

<!-- 禁止百度对网页转码(百度的Siteapp页面转码) -->
<meta http-equiv="Cache-Control" content="no-siteapp" />

<!-- 页面生成器 -->
<meta name="generator" content="hexo" />

<!-- 页面修改信息 -->
<meta name="revised" content="story,2015/07/22" />

<!-- 版权信息 -->
<meta name="copyright" content="All Rights Reserved" />

<!-- 页面爬虫设置 -->
<meta name="robots" content="index,follow" />
<!-- robots的content取值 -->
<!-- all：文件将被检索，且页面上的链接可以被查询 -->
<!-- none：文件将不被检索，且页面上的链接不可以被查询 -->
<!-- index：文件将被检索 -->
<!-- follow：页面上的链接可以被查询 -->
<!-- noindex：文件将不被检索，但页面上的链接可以被查询 -->
<!-- nofollow：文件将被检索，但页面上的链接不可以被查询 -->

<!-- 声明搜索引擎抓取间隔 -->
<meta name="revisit-after" content="10 days" />

```

### http-equiv

```html
<!-- 字符编码 -->
<meta http-equiv="content-type" content="text/html;charset=UTF-8" />
<!-- html5写法 -->
<meta charset="utf-8" />


<!--告诉IE8+使用最新的引擎渲染网页，chrome=1则可以激活Chrome Frame-->
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">

<!-- 打开和关闭 DNS 预读取： on/off  -->
<!-- 默认情况下，通过 HTTPS 加载的页面上内嵌链接的域名并不会执行预加载 -->
<!-- https://developer.mozilla.org/zh-CN/docs/Controlling_DNS_prefetching -->
<meta http-equiv="x-dns-prefetch-control" content="on">
<!-- 与以下link标签配合使用 -->
<link rel="dns-prefetch" href="http://www.spreadfirefox.com/">

<!--语言-->
<meta http-equiv="content-language" content="zh-CN" />

<!-- 指定网页在缓存中的过期时间 （格式必须为GMT格式或0）-->
<meta http-equiv="Expires" content="0" />
<meta http-equiv="Expires" content="Wed, 26 Feb 1997 08:21:57 GMT" />

<!--控制页面缓存（不缓存）-->
<meta http-equiv="Expires" content="0" />
<!-- HTTP/1.0 -->
<meta http-equiv="Pragma" content="no-cache" />
<!-- HTTP/1.0（可选值：public,no-cache,no-store）-->
<meta http-equiv="Cache-Control" content="no-cache">

<!-- 页面多少秒后刷新或多少秒后跳转到其他网页 -->
<meta http-equiv="refresh" content="30" />
<meta http-equiv="refresh" content="0;URL= " />

<!--Window-target (显示窗口的设定) 强制页面在当前窗口以独立页面显示-->
<meta http-equiv="Widow-target" content="_top" />
<!--这个属性是用来防止别人在框架里调用你的页面。Content选项：_blank、_top、_self、_parent。-->

<!-- cookie设置 -->
<meta http-equiv="set-cookie" content="cookie value=xxx;expires=Wed,22-Jul-201511:11:11GMT；path=/" />

<!-- 脚本类型 -->
<meta http-equiv="Content-Script-Type" content="text/javascript">

<!-- 禁止从本地缓存中读取页面 -->
<meta http-equiv="Pragma"content="no-cache"> 
```

### 移动端

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0,maximum-scale=1.0, user-scalable=no"/>
<!-- viewport的content取值 -->
<!-- width：宽度（数值 / device-width）（200~10000，默认为980px） -->
<!-- height：高度（数值 / device-height）（223~10000） -->
<!-- initial-scale：初始缩放比例 （0~10） -->
<!-- minimum-scale：允许用户缩放到的最小比例 -->
<!-- maximum-scale：允许用户缩放到的最大比例 -->
<!-- user-scalable：是否允许用户缩放 (no/yes)  -->
 
<!-- uc强制竖屏 -->
<meta name="screen-orientation" content="portrait">
<!-- QQ强制竖屏 -->
<meta name="x5-orientation" content="portrait">
<!-- UC强制全屏 -->
<meta name="full-screen" content="yes">
<!-- QQ强制全屏 -->
<meta name="x5-fullscreen" content="true">
<!-- UC应用模式 -->
<meta name="browsermode" content="application">
<!-- QQ应用模式 -->
<meta name="x5-page-mode" content="app">
 
 
<!-- 声明添加到主屏幕的Web App标题（ios safari;android只能通过title标签声明） -->
<meta name="apple-mobile-web-app-title" content="Web App名称" />

<!-- 声明添加到主屏幕时隐藏地址栏和状态栏（即全屏）iOS 和 Android5.0+ 上都通用 -->
<meta name="apple-mobile-web-app-capable" content="yes" />

<!-- IOS全屏模式下隐藏状态栏/设置状态栏颜色 -->
<meta name="apple-mobile-web-app-capable" content="yes" />

<!-- 有三个固定值可选：
    default： 白色，网页从状态栏以下开始显示
    black：黑色，网页从状态栏以下开始显示
    black-translucent：灰色半透明，网页将充满整个屏幕，状态栏会盖在网页之上
-->
<meta name="apple-mobile-web-app-status-bar-style" content="black" />

<!-- IOS添加智能 App 广告条 Smart App Banner -->
<meta name="apple-itunes-app" content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL">
 
<!-- 去除iphone 识别数字为号码 -->
<meta name="format-detection" content="telephone=no">
<!-- 拨打电话 -->
<a href="tel:10086">客服电话</a>
<!-- 发送短信 -->
<a href="sms:10086">发送短信</a>

<!-- 不识别邮箱 -->
<meta name="format-detection" content="email=no">
<!-- 发送邮件 -->
<a href="mailto:zhengji@gmail.com">发送邮件</a>


<!-- 禁止跳转至地图 -->
<meta name="format-detection" content="adress=no">
<!-- 可以连写-->
<meta name="format-detection" content="telephone=no,email=no,adress=no">

<!-- 添加智能 App 广告条 Smart App Banner（iOS 6+ Safari） -->
<meta name="apple-itunes-app" content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL">

<!-- uc强制竖屏 -->
<meta name="screen-orientation" content="portrait">
<!-- QQ强制竖屏 -->
<meta name="x5-orientation" content="portrait">
<!-- UC强制全屏 -->
<meta name="full-screen" content="yes">
<!-- QQ强制全屏 -->
<meta name="x5-fullscreen" content="true">
<!-- UC应用模式 -->
<meta name="browsermode" content="application">
<!-- QQ应用模式 -->
<meta name="x5-page-mode" content="app">
```

#### iOS图标
`<link>`标签，`rel` 参数： `apple-touch-icon` 图片自动处理成圆角和高光等效果。
`apple-touch-icon-precomposed` 禁止系统自动添加效果，直接显示设计原图。
- **iPhone 和 iPod touch**，默认 `57×57px`  
```
<link rel="apple-touch-icon-precomposed" href="/apple-touch-icon-57x57-precomposed.png" />
```
- **iPad**，`72×72px`  
```
<link rel="apple-touch-icon-precomposed" sizes="72x72" href="/apple-touch-icon-72x72-precomposed.png" />
```

- **Retina iPhone**, `114×114px`
```
<link rel="apple-touch-icon-precomposed" sizes="114x114" href="/apple-touch-icon-114x114-precomposed.png" />
```

- **Retina iPad**，`144×144px`
```
<link rel="apple-touch-icon-precomposed" sizes="144x144" href="/apple-touch-icon-144x144-precomposed.png" />
```

- iOS 图标大小在iPhone 6+上是180×180，iPhone 6 是120×120
```
<!-- 适配iPhone 6+ -->
<link rel="apple-touch-icon-precomposed" sizes="180x180" href="retinahd_icon.png">
```

#### 添加RSS订阅
```
<link rel="alternate" type="application/rss+xml" title="RSS" href="/rss.xml" />
```


**参考**：
- [html meta标签之http-equiv](http://ziren.org/html-css/html-meta-http-equiv.html)
- [移动前端第二弹：善用meta](http://blog.doyoe.com/2015/10/20/mobile/%E7%A7%BB%E5%8A%A8%E5%89%8D%E7%AB%AF%E7%AC%AC%E4%BA%8C%E5%BC%B9%EF%BC%9A%E5%96%84%E7%94%A8meta/)
- [HTML5、移动开发meta标签](https://github.com/ccforward/cc/issues/8)