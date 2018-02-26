## 属性
### 元素访问
#### 集合类

代码 | 集合ji | 集合类型 | 元素类型
---|---|---|---
forms | 所有 `form` 元素的集合 | HTMLCollection | HTMLFormElement
images | 所有`image`元素的集合 | HTMLCollection | HTMLImageElement
links | 所有具有`href` 属性值的 `<area>` 元素 `<a>` 元素的集合 | HTMLCollection | HTMLAnchorElement
anchors|拥有`name`属性的`a`元素的集合 | HTMLCollection | HTMLAnchorElement
scripts| 所有`<script>`元素的集合 | HTMLCollection | HTMLScriptElement
styleSheets | 文档中链接或嵌入的样式表集合 | StyleSheetList |  CSSStyleSheet


**注意**：

- 以上集合类的单个元素可通过方法`item(index)`或下表索引`[index]`访问
- `forms`集合中的单个元素还可通过`document[form的name属性值]`获取；`form`中的表单元素可通过`formObj[元素的name属性值/id属性值]`获取；`formObj.elements`:获取所有数据采集元素（带name属性）;`formObj.elements[i/name属性]`：获取表单中任意一个数据采集元素。(如果多个元素，使用同一name属性，就返回所有元素的数组;不能把提交按钮的name或者id设置为submit)

#### 元素类

代码 | 返回值 | 返回值类型
---|---|---
head | （IE9+）返回当前文档中的 `<head>` 元素。如果有多个 `<head>` 元素，则返回第一个 | HTMLHeadElement
body|返回或设置当前文档中的`<body>`元素或者最外层`<frameset>`元素| HTMLBodyElement或
documentElement | 返回文档对象（document）的根元素（例如，HTML文档的 <html> 元素）|
activeElement | 返回当前页面中获得焦点的元素 | 
defaultView | （IE9+）返回当前 document 对象所关联的 window 对象，如果没有，会返回 null | 

**参考**：
- https://developer.mozilla.org/zh-CN/docs/Web/API/Document