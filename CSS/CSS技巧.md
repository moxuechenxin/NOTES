## 利用`CSS`的`content`属性`attr`抓取资料

```html
<div data-msg="Open this file in Github Desktop">  
  hover
</div>
```

```css
div{
  width:100px;
  border:1px solid red;  
  position:relative;
}
div:hover:after{
  content:attr(data-msg);
  position:absolute;
  font-size: 12px;
  width:200%;
  line-height:30px;
  text-align:center;
  left:0;
  top:25px;
  border:1px solid green;
}
```

## 利用 `:valid` 和 `:invalid` 来做表单即时校验
- `:required` 伪类指定具有required 属性的表单元素
- `:valid` 伪类指定一个通过匹配正确的所要求的表单元素
- `:invalid` 伪类指定一个不匹配指定要求的表单元素

## 利用 `nth-of-type` 选择某范围内的子元素

```css
/* 选择5-10的子元素 */
table tr:nth-child(n+5):nth-child(-n+10) {
    background-color: red;
}

/* 红白相间 */
tbody tr:nth-of-type(2n) {  
  background-color: red;
}
tbody tr:nth-of-type(2n+1) {
  background-color: green;
}
```

## 为破碎图象定义样式
```css
img {  
  display: block;
  font-family: Helvetica, Arial, sans-serif;
  font-weight: 300;
  height: auto;
  line-height: 2;
  position: relative;
  text-align: center;
  width: 100%;
}
```
以添加伪元素的法则来显示用户信息和URL的引用
```css
img::before {  
  content: "We're sorry, the image below is broken :(";
  display: block;
  margin-bottom: 10px;
}

img::after {  
  content: "(url: " attr(src) ")";
  display: block;
  font-size: 12px;
}
```

##  文字竖向排列（writing-mode）
参考：[改变CSS世界纵横规则的writing-mode属性](http://www.zhangxinxu.com/wordpress/2016/04/css-writing-mode/)
```css
.verticle-mode {
    writing-mode: tb-rl;
    -webkit-writing-mode: vertical-rl;      
    writing-mode: vertical-rl;
}
/* IE7比较弱，需要做点额外的动作 */
.verticle-mode {
    *width: 120px;
}
.verticle-mode h4,
.verticle-mode p {
    *display: inline;
    *writing-mode: tb-rl;
}
.verticle-mode h4 {
    *float:right;
}
```

## 实现鼠标悬浮内容自动撑开的过渡动画
> 需要为一个列表添加个动画，容器的高度是不确定的，也就是高度为 auto，悬浮时候撑开内容有个过渡动画

由于高度的不确定，而 `transtion` 是需要具体的数值，所以设置 `height:auto` 是无法实现效果的，可以通过 `max-height` 这个属性间接的实现这么个效果

```html
<ul>
  <li>
    <div class="hd"> 列表1 </div>
    <div class="bd">列表内容<br>内容列表内容<br>内容列表内容<br>内容</div>
  </li>
  <li>
    <div class="hd"> 列表1 </div>
    <div class="bd">列表内容<br>内容列表内容<br>内容列表内容<br>内容</div>
  </li>
  <li>
    <div class="hd"> 列表1 </div>
    <div class="bd">列表内容<br>内容列表内容<br>内容列表内容<br>内容</div>
  </li>
</ul>
```

```css
.bd {
  max-height:0;
  overflow:hidden;
  transition: all 1s ease-out;
}
li:hover .bd {
  max-height: 600px;
  transition-timing-function: ease-in;
}
```

## 移动web页面支持弹性滚动
> 在IOS机型中，非body元素的滚动条会非常不流畅，又不想用JS模拟滚动条

```css
body {
  -webkit-overflow-scrolling: touch; /* ios5+ */
}
* {
  overflow:auto;
}
```

## 实现文字两端对齐
`text-align-last: justify;`或`text-align: justify;`

## 改变 input 焦点光标的颜色（`caret-color`）

```html
<input value="This field uses a default caret." />
<input class="custom" value="I have a custom caret color!" />
```

```css
input {
  caret-color: auto;
  display: block;
  margin-bottom: .5em;
}

input.custom {
  caret-color: red;
}
```

## 让网站所有图片变成黑白色彩的

```css
img.desaturate {
  filter: grayscale(100%);
  -webkit-filter: grayscale(100%);
  -moz-filter: grayscale(100%);
  -ms-filter: grayscale(100%);
  -o-filter: grayscale(100%);
}
```

## border-radius

```css
.box {
  border-radius: 5px 5px 3px 2px / 5px 5px 1px 3px;
}
```
斜线前面的影响的是水平方向，斜线后面影响的是垂直方向，各个数字就分别代表四个不一样的方向

## outline-offset

```css
input {
  outline-offset: 4px;
}
```
设置默认线框的距离

## empty-cells

```css
table {
  empty-cells: hide;
}
```
将table元素中的空单元格隐藏起来

## currentColor

```css
.foo{
  color: #f00;
  border: solid 2px currentColor;
}
```
这个属性值相当于一个变量，其实际值与元素的color值一致

- - -
**参考**：
- [CSS 黑魔法小技巧，让你少写不必要的JS，代码更优雅](https://github.com/jawil/blog/issues/29)
- [CSS 专业技巧](https://github.com/AllThingsSmitty/css-protips/tree/master/translations/zh-CN#%E6%94%AF%E6%8C%81%E6%83%85%E5%86%B5)
- [你不知道的CSS（三）](https://segmentfault.com/a/1190000011194809)
- [谈谈一些有趣的 CSS 话题](https://github.com/chokcoco/iCSS)
