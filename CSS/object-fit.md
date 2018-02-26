## 概述
**兼容性**：IE不支持

`object-fit`属性指定了替换元素的内容应该如何使用他的宽度和高度来填充其容器  
`object-position`属性指定了替换元素在容器中的对齐方式

类似`background-size`和`background-position`

## object-fit
`object-fit`主要适合于替换元素，比如：`<video>`、`<object>`、`<img>`、`<input type="image">`、`<svg>`、`<svg:image>`和`<svg:video>`等

**取值**：
- `fill`： **默认值**。填充，可替换元素填满整个内容区域，可能会改变长宽比，导致拉伸。
- `contain`：包含，保持原始的尺寸比例，保证可替换元素完整显示，宽度或高度至少有一个和内容区域的宽度或高度一致，部分内容会空白。
- `cover`：覆盖，保持原始的尺寸比例，保证内容区域被填满。因此，可替换元素可能会被切掉一部分，从而不能完整展示。
-  `none`：保持可替换元素原尺寸和比例。当图片尺寸设置的比原始尺寸小时，图片原点不动，超出部分会自动截取；当图片尺寸设置比原始尺寸大时，四周会留白。
- `scale-down`：等比缩小。就好像依次设置了none或contain, 最终呈现的是尺寸比较小的那个。

## object-position
`object-position`属性决定了它的盒子里面替换元素的对齐方式。取值同`background-size`，默认值为`50% 50%`。

**参考**：
- [CSS3 Object-fit和Object-position](http://www.w3cplus.com/css3/css3-object-fit-and-object-position-properties.html)
- [聊聊 CSS3 中的 object-fit 和 object-position](http://web.jobbole.com/89003/)