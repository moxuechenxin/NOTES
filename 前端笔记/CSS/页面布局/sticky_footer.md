## 布局描述
如果页面内容不足够长时，页脚固定在浏览器窗口的底部；如果内容足够长时，页脚固定在页面的最底部

## 实现方案
HTML结构:
```html
<div class="wrapper">
    <div class="content"><!-- 页面主体内容区域 --></div>
    <div class="footer"><!-- 需要做到 Sticky Footer 效果的页脚 --></div>
</div>
```

### 利用css

#### 1. absolute

```scss
$footer_h: 50px;

html, body {
    height: 100%;
}
.wrapper {
    position: relative;
    min-height: 100%;
    padding-bottom: $footer_h;
    box-sizing: border-box;
}
.footer {
    position: absolute;
    bottom: 0;
    height: $footer_h;
}
```

#### 2. calc
通过计算函数 calc 计算（视窗高度 - 页脚高度）赋予内容区最小高度

```scss
$footer_h: 50px;

.content {
    min-height: calc(100vh - $footer_h);
}
.footer {
    height: $footer_h;
}
```

#### 3. table

```scss
html, body {
    height: 100%;
}
.wrapper {
    display: table;
    width: 100%;
    min-height: 100%;
}
.content {
    display: table-row;
    height: 100%;
}
```

注意：使用 table 方案存在一个比较常见的样式限制，通常 margin、padding、border 等属性会不符合预期，别把其他样式写在 table 上

#### 4. Flexbox

```scss
html {
    height: 100%;
}
body {
    min-height: 100%;
    display: flex;
    flex-direction: column;
}
.content {
    flex: 1;
}
```

**参考**：
- [Sticky Footer，完美的绝对底部]()
