## 属性概览
### 前置属性：
- transform-origin
- transform-style
- perspective
- perspective-origin
- backface-visibility

### 变形（transform）：
- translate
- rotate
- scale
- skew
- matrix


## 前置属性
### transform-origin  
用来设置元素的运动的基点（参照点）。默认点是元素的中心点

**语法**：`transform-origin: x-axis y-axis z-axis;`

**默认值**：`50% 50% 0`

**注意**：使用此属性必须先使用 `transform` 属性  

**参数**：
- `x-axis`  
定义视图被置于 X 轴的何处  
**可选值**：left、center、right、<length>、%

- `y-axis`
定义视图被置于 Y 轴的何处  
**可选值**：top、center、bottom、<length>、%

- `z-axis`  
定义视图被置于 Z 轴的何处：<length>

```css
#div2 {
    padding:50px;
    position: absolute;
    border: 1px solid black;
    background-color: yellow;
    -ms-transform: rotate(45deg); /* IE 9 */
    -ms-transform-origin:20% 40%; /* IE 9 */
    -webkit-transform: rotate(45deg); /* Safari and Chrome */
    -webkit-transform-origin:20% 40%; /* Safari and Chrome */
    -moz-transform: rotate(45deg); /* Firefox */
    -moz-transform-origin:20% 40%; /* Firefox */
    -o-transform: rotate(45deg); /* Opera */
    -o-transform-origin:20% 40%; /* Opera */
    transform: rotate(45deg);
    transform-origin:20% 40%;
}
```

### [transform-style](http://www.runoob.com/cssref/css3-pr-transform-style.html)
（IE11+）规定如何在 3D 空间中呈现被嵌套的元素  
**注意**：
1. 该属性必须与 `transform` 属性一同使用。
2. 在变形元素自身上指定该属性是没有用的，它用于指定舞台，所以要**在变形元素的父元素上设置该属性**。设定后，所有子元素共享该舞台。
3. 如果同时设了`transform-style: preserve-3d`和`overflow: hidden`，3D效果将失效，等价于`transform-style: flat`

**厂商前缀**：  
`-webkit-`，`-ms-`,`-moz-`


**可选值**：
值	| 描述
--|--
flat  |	**默认值**。子元素将不保留其 3D 位置。
preserve-3d |	子元素将保留其 3D 位置。

### [perspective](http://www.runoob.com/cssref/css3-pr-perspective.html)
（IE10+）指定3D的视距。（可指定负值）

**厂商前缀**：  
`-webkit-`,`-moz-`,`-ms-`

**可选值**：
值	| 描述
--|--
none |	**默认值**。与 0 相同。不设置透视。
number |	元素距离视图的距离，以像素计。

**两种用法**：
- **用在舞台元素上（变形元素们的共同父辈元素）**：
```css
.stage {
    perspective: 600px;
}
```

- **用在当前变形元素上，与transform的其他属性写在一起**：
```css
.stage .item {
    transform: perspective(600px) rotateY(45deg);
}
```

**注意**：  
- `perspective` 属性只影响 3D 转换元素。
- 请与 `perspective-origin` 属性一同使用该属性，这样您就能够改变 3D 元素的底部位置。

### [perspective-origin](http://www.runoob.com/cssref/css3-pr-perspective-origin.html)
（IE10+）设置视距的基点（变形元素父元素设置该属性）

**厂商前缀**：  
`-webkit-`,`-moz-`,`-ms-`

**语法**：
```css
perspective-origin: x-axis y-axis;
```
**默认值**：`50% 50%;`  
**可选值**：
- **x-axis**：`left`,`center`,`right`,`<length>`,`%`
- **y-axis**：`top`,`center`,`bottom`,`<length>`,`%`

### [backface-visibility](http://www.runoob.com/cssref/css3-pr-backface-visibility.html)
（IE10+）定义当元素不面向屏幕时是否可见 (例如：绕x轴或y轴旋转角度超过90deg)  
**厂商前缀**：  
`-webkit-`,`-moz-`

**可选值**：
值|描述
--|--
visible |	**默认值**。背面是可见的。
hidden	| 背面是不可见的。

## transform
参考：http://www.w3cplus.com/content/css3-transform  

transform往往与transition和@keyframes配合使用实现动画效果

**兼容性**：  
IE10+, Firefox,Chrome 和 Opera支持  `transform-origin` 属性。  
IE9 支持替代的 `-ms-transform-origin` 属性（仅适用于 2D 转换）。  
Safari 和 Chrome 支持替代的 `-webkit-transform-origin` 属性（3D 和 2D 转换）    
Opera 只支持 2D 转换  

**语法**：
`transform: none|transform-functions;`    
**多个转换间用空格分隔**

transform-functions:主要有  **translate（位移）**、**rotate（旋转）**、**scale（缩放）**、**skew（扭曲/歪斜）**、  **matrix（矩阵）**

**默认值**：  
`none;`

### translate 位移
**2D**：
- `translate(dis_x,dis_y)`
- `translateX(dis_x)` 等同于 `translate(dis_x)`
- `translateY(dis_y)`

**3D**：
- `translate3d(dis_x,dis_y,dis_z)`
- `translateZ(dis_z)`

**注意**：
- `dis_z`只能为px值，可以为负
- `dis_z`越大表示离眼睛越近，元素就越大，但当值大于`perspective`视距时元素将消失

### rotate 旋转
**2D**：
- `rotate(30deg)`  
**注意**：正数表示顺时针旋转，负数表示逆时针旋转。

**3D**：
- `rotate3d(x,y,z,a)` a表示3D舞台上旋转的角度
- `rotateX`
- `rotateY`
- `rotateZ`

### scale 缩放
**2D**：
- `scale(2,3)` 
- `scale(2)`等同于`scale(2,2)`
- `scaleX(2)`
- `scaleY(3)`

**注意**：
- 小数代表缩小，大于1的数表示放大
- 负数代表先反转再缩放

**3D**：
- `scale3d(sx,sy,sz)`
- `scaleZ(sz)`

### skew 扭曲
**2D**：
- `skew(30deg, 60deg)`
- `skew(30deg)`等同于`skew(30deg,0)`
- `skewX(30deg)`
- `skewY(60deg)`

### matrix
**参考**：http://www.jianshu.com/p/52e0018e6ce2

## 3D转换实例
**html**:
```html
<div class="stage">
    <div class="item"></div>
</div>
```
**css : 1**
```css
.stage{
    perspective:200px;
    transform-style: preserve-3d;
    perspective-origin:50% 50%;
}
.item{
    transform-origin:50% 50% 0;
    transform:translateZ(100px);
}
```

**css : 2**
```css
.stage{
    
}
.item{
    transform-origin:50% 50% 0;
    transform:perspective(200px) translateZ(100px);
}
```

**参考**：
- http://www.jianshu.com/p/17e289fcf467
- http://www.zhangxinxu.com/wordpress/2012/09/css3-3d-transform-perspective-animate-transition/