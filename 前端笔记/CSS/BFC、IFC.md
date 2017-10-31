## BFC（Block formatting contexts）块格式化上下文
### 生成条件（四选一）
- 浮动
- 绝对定位
- 非块盒的块容器（inline-block）
- overflow不为visible的块盒

### 特性
- 从包含块顶部竖直方向排列
- BFC可以阻止margin合并（BFC内部兄弟盒子之间的margin会合并——可以通过把兄弟之一变成BFC解决合并）

## IFC（Inline formatting contexts）内联格式化上下文
### 生成条件：只有在一个块级元素中仅仅包含内联级别元素时才会生成
### 特性
- 从包含块顶部水平方向排列
- 排列情况和浮动与否会改变行盒的高度
- 当一个行盒被分割，`margin`, `border`, `padding`都不会再有视觉效果了

**参考**：
- [CSS原理解析之模型篇](https://juejin.im/post/59dca82051882578db27b1d6)
- [我对BFC的理解](http://www.cnblogs.com/dojo-lzz/p/3999013.html)
