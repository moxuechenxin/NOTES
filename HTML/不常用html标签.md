## 语义化标签
### \<header\> (h5)
`<header>`元素描述了文档的头部区域
`<header>`元素也用于定义内容的介绍展示区域.
在页面中你可以使用多个`<header>`元素

```html
<!-- 该实例定义了文章的头部 -->
<article>
  <header>
    <h1>Internet Explorer 9</h1>
    <p><time pubdate datetime="2011-03-15"></time></p>
  </header>
  <p>
    Windows Internet Explorer 9(缩写为 IE9 )是在2011年3月14日21:00发布的
  </p>
</article>
```

### \<footer\> (h5)
`<footer>`元素描述了文档的底部区域
`<footer>`元素应该包含它的包含元素
一个页脚通常包含文档的作者，著作权信息，链接的使用条款，联系信息等
文档中可以使用多个`<footer>`元素

```html
<footer>
  <p>Posted by: Hege Refsnes</p>
  <p><time pubdate datetime="2012-03-01"></time></p>
</footer>
```

### \<nav\> (h5)
`<nav>`标签定义导航链接的部分  
`<nav>` 元素用于定义页面的导航链接部分区域，但是，不是所有的链接都需要包含在 `<nav>` 元素中!

```html
<nav>
  <a href="/html/">HTML</a> |
  <a href="/css/">CSS</a> |
  <a href="/js/">JavaScript</a> |
  <a href="/jquery/">jQuery</a>
</nav>
```

### \<section\> (h5)
`<section>`标签定义文档中的节（section、区段）。比如章节、页眉、页脚或文档中的其他部分。
根据W3C HTML5文档: section 包含了一组内容及其标题

```html
<section>
  <h1>WWF</h1>
  <p>The World Wide Fund for Nature (WWF) is....</p>
</section>
```

### \<article\> (h5)
`<article>`标签定义独立的内容
`<article>`元素使用实例：
- Forum post
- Blog post
- News story
- Comment

```html
<article>
  <h1>Internet Explorer 9</h1>
  <p>Windows Internet Explorer 9(缩写为 IE9 )在2011年3月14日21:00 发布。</p>
</article>
```

### \<aside\> (h5)
`<aside>`标签定义页面主区域内容之外的内容（比如侧边栏）。
`<aside>`标签的内容应与主区域的内容相关

```html
<p>My family and I visited The Epcot center this summer.</p>

<aside>
  <h4>Epcot Center</h4>
  <p>The Epcot Center is a theme park in Disney World, Florida.</p>
</aside>
```

### \<figure\> 和 \<figcaption\> (h5)
`<figure>`：规定独立的流内容（图像、图表、照片、代码等等），其内容应该与主内容相关，但如果被删除，则不应对文档流产生影响。
`<figcaption>`：定义 `<figure>` 元素的标题，应该被置于`<figure>` 元素的第一个或最后一个子元素的位置。

```html
<figure>
  <img src="img_pulpit.jpg" alt="The Pulpit Rock" width="304" height="228">
  <figcaption>Fig1. - The Pulpit Pock, Norway.</figcaption>
</figure>
```

### \<dl\> 、\<dt\>和\<dd\>
`<dl>` 标签定义了定义列表（definition list）
`<dl>` 标签用于结合 `<dt>`（定义列表中的项目：标题）和 `<dd>` （描述列表中的项目）

```html
<dl>
  <dt>计算机</dt>
  <dd>用来计算的仪器 ... ...</dd>
  <dt>显示器</dt>
  <dd>以视觉方式显示信息的装置 ... ...</dd>
</dl>
```

## 其它
### \<embed\> (h5)
`<embed>`标签定义嵌入的内容，比如插件。

属性 | 值 | 描述
---|---|---
height	| *pixels*	| 设置嵌入内容的高度。
src	| *url*	| 嵌入内容的 URL。
type	| *type*	| 定义嵌入内容的类型。
width	| *pixels*	| 设置嵌入内容的宽度。

```html
<embed
  src="/i/helloworld.swf"
  type="application/x-shockwave-flash"
  allowFullScreen="true"
  quality="high"
  width="720"
  height="600"
  align="middle"/>
```

### [\<object\>](http://www.runoob.com/tags/tag-object.html)
`<object>` 标签用于包含对象，比如图像、音频、视频、Java applets、ActiveX、PDF 以及 Flash

```html
<object
  type="application/x-shockwave-flash"
  data="helloworld.swf"
  width="400"
  height="400">
```

**参考**：
- [菜鸟教程：HTML5 语义元素](http://www.runoob.com/html/html5-semantic-elements.html)
