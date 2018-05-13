## Formatting context(格式化上下文)
W3C CSS2.1 规范中的一个概念，它是页面中的一块渲染区域，并且有一套渲染规则.
它决定了其子元素将如何定位，以及和其他元素的关系和相互作用

## BFC（Block formatting contexts）块格式化上下文
 一个块级元素的渲染显示规则，容器里面的子元素不会影响到外面的元素，反之也如此

### 生成条件（满足任一条件）
- body 根元素；
- 浮动元素：float 不为none的属性值；
- 绝对定位元素：position (absolute、fixed)
- display为： inline-block、table-cells、flex
- overflow 除了visible以外的值 (hidden、auto、scroll)

### 特性
- 内部的盒子会在垂直方向，一个个地放置；
- BFC是页面上的一个隔离的独立容器；
- 属于同一个BFC的 两个相邻Box的 上下margin会发生重叠 ；
- 计算BFC的高度时，浮动元素也参与计算
- 每个元素的左边，与包含的盒子的左边相接触，即使存在浮动也是如此；
- BFC的区域不会与float重叠

## IFC（Inline formatting contexts）内联格式化上下文
### 生成条件：只有在一个块级元素中仅仅包含内联级别元素时才会生成
### 特性
- 从包含块顶部水平方向排列
- 排列情况和浮动与否会改变行盒的高度
- 当一个行盒被分割，`margin`, `border`, `padding`都不会再有视觉效果了

### 主要影响IFC内布局的css：
- font-size
- line-height
- height
- vertical-aligin

- - -
**参考**：
- [理解BFC、IFC、GFC、FFC](http://blog.csdn.net/u011472830/article/details/73010596)
- [CSS原理解析之模型篇](https://juejin.im/post/59dca82051882578db27b1d6)
- [我对BFC的理解](http://www.cnblogs.com/dojo-lzz/p/3999013.html)
