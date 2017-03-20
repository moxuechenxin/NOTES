## html相关标签
### `<frameset>`和`<frame>`
参考：http://www.w3school.com.cn/tags/tag_frameset.asp    

\- frameset 元素可定义一个框架集。它被用来组织多个窗口（框架）。  
\- 每个框架存有独立的文档。  
\- 在其最简单的应用中，frameset 元素仅仅会规定在框架集中存在多少列或多少行。您必须使用 cols 或 rows 属性。


```html
<html>

<frameset cols="25%,50%,25%">

  <frame src="/example/html/frame_a.html">
  <frame src="/example/html/frame_b.html">
  <frame src="/example/html/frame_c.html">

</frameset>

</html>
```

### `<iframe>`
参考：http://www.w3school.com.cn/tags/tag_iframe.asp  

**注意**：iframe元素有默认宽高，属于内联元素

### 几个window对象

- **top**：指向最高（最外）层的框架，也就是浏览器窗口
- **parent**：指向当前框架的直接上层框架
- **self**：指向当前框架的window对象

## 相关元素
### `<a>`
参考：http://www.w3school.com.cn/tags/tag_a.asp  

a 的 target 属性可以是框架名（name属性值），即在指定frame内打开链接文档
