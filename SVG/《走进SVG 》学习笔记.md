## 1.1 SVG简介
- SVG是使用XML的矢量文件
- W3C标准(1.1)：http://www.w3.org/TR/SVG11/
- 浏览器支持情况: IE9+

###  使用方式:
- 浏览器直接打开
- 在HTML中使用`<img>`标签引用
- 直接在HTML中使用SVG标签
- 作为CSS背景

## 1.2 基本图形和属性
**基本图形**：  
`<rect>`、`<circle>`、`<ellipse>`、`<line>`、`<polyline>`、`<polygon>`

**基本属性**：  
`fill`、`stroke`、`stroke-width`、`transform`

#### `<rect>` 矩形
**属性**：
- `x`、`y`：距离坐标原点的距离（左上角）
- `width`、`height`：矩形宽高
- `rx`、`ry`：矩形圆角（如果只定义其一，则另一属性值相同）

#### `<circle>` 圆
**属性**：
- `cx`、`cy`：圆心距离坐标原点的距离
- `r`： 半径

#### `<ellipse>` 椭圆
**属性**：
- `cx`、`cy`：中心点距离坐标原点的距离
- `rx`、`ry`：两个方向的半径

#### `<line>` 直线
**属性**：
- `x1`、`y1`：起点坐标
- `x2`、`y2`：终点坐标

#### `<polyline>` 折线
**属性**：
- `points`：点坐标集合（用空格或,分隔）

```
points="x1 y1 x2 y2 x3 y3 x4 y4 ..."
```

#### `<polygon>` 多边形
**属性**：
- `points`：点坐标集合（首尾点相连图形闭合）

```
points="x1 y1 x2 y2 x3 y3 x4 y4 ..."
```
 
### 填充、描边和变换
- `fill`：默认为`#000`，填充颜色
- `stroke`： 描边的颜色
- `stroke-width`： 描边粗细
- `transform`：变换

## 1.3 基本操作API
**创建图形**：  
`document.createElementNS(ns, tagName)`

`ns`: 命名空间

**添加图形**：  
`element.appendChild(childElement)`

**设置/获取属性**：  
`element.setAttribute(name, value)`    
`element.getAttribute(name)`

# 2 SVG的坐标系统与坐标变换
## 2.1 SVG的世界、视野(viewBox)、视窗的概念
**视野与世界**：
- 世界是无穷大的
- 视野是观察世界的一个矩形区域

**世界、视野(viewbox)、视窗**：
- width、height：控制视窗（viewport）
- SVG代码：定义世界
- [viewBox、preserveAspectRatio](http://www.htmleaf.com/ziliaoku/qianduanjiaocheng/201506182064.html)：控制视野

```xml
<svg xmlns="..."
  width="800" height="600"
  viewBox="0 0 400 300"
  preserveAspectRatio="xMidYMid meet">
  <!-- SVG content -->
</>
```

**viewBox**：  
定义视野  
**语法**：`viewBox = "<x> <y> <width> <height>"`  

**注**：
- 如果没有指定`viewBox`属性，则同viewport
- 如果宽高比和viewport相同，将会拉伸填充满整个viewport
- 如果宽高比和viewport不同，使用`preserveAspectRatio`控制显示行为

**preserveAspectRatio**：  
**语法**：preserveAspectRatio = [defer] <align> [meetOrSlice]  
**参数**：
- `defer`: 可选，仅在`image`元素上应用`preserveAspectRatio`属性时才使用 
- `align`: 默认值`xMidYMid`，指定`viewbox`在viewport中的对齐方式（除了`none`其余值由两部分组成：X方向的对齐方式、Y方向的对齐方式）

取值 | 描述
---|---
none | 图形会被缩放以适应viewport大小(不再保持宽高比)
xMin | viewBox的最小X值对齐viewport的左边部
xMid | viewBox的X轴中点对齐viewport的X轴中点
xMax | viewBox的最大X值对齐viewport的右边部
YMin | viewBox的最小Y值对齐viewport的顶边
YMid | viewBox的Y轴中点对齐viewport的Y轴中点
YMax | viewBox的最大Y值对齐viewport的底边

- `meetOrSlic`：类似`backgroundSize`


取值 | 描述
---|---
meet	| 默认值, 保持宽高比并将viewBox按长宽比中的较小值缩放(类似background-size的contain)
slice |	保持宽高比并将viewBox按长宽比中的较大值缩放(类似background-size的cover)
none	| 不保存宽高比。缩放图像适合整个viewbox，可能会发生图像变形

**DEMO**：

```xml
<svg width="500" height="75" viewBox="0 0 250 75"
     preserveAspectRatio="xMinYMin meet"
     style="border: 1px solid #cccccc;">
 
    <rect x="1" y="1" width="50" height="50"
          style="stroke: #000000; fill:none;"/>
</svg>   
```
**说明**：viewport的尺寸为`500 * 75`， viewBox为 `0 0 250 25`，X轴方向的比值为 500 / 250 = 2, Y轴为 75 / 25 = 3, 则`meetOrSlic`为`meet`时，viewBox长宽都缩放2倍；为`slice`时，缩放3倍；为`none`时，宽度缩放2倍，长度缩放3倍（效果就是viewBox相关方向的尺寸也跟着缩放）

## 2.2 SVG中的图形分组
**知识点**：
- `<g>`标签创建分组
- 属性继承
- `transform`属性定义坐标变换
- 可以嵌套使用

```xml
<svg xmlns="...">
  <g stroke="green" 
    fill="none" 
    transform="translate(0, 50)">
    <rect 
      x="100" 
      y="50"
      width="100"
      height="50">
    </rect>
    <rect 
      x="140" 
      y="100"
      width="20"
      height="120">
    </rect>
  </g>
</svg>
```

## 2.3 坐标系统
- 笛卡尔直角坐标系
- 原点
- 互相垂直的两条数轴
- 角度定义（顺时针为正值）

## 2.4 四个坐标系
- **用户坐标系（原始坐标系）**：世界的坐标系
- **自身坐标系**：每个图形元素或分组独立与生俱来
- **前驱坐标系**：父容器的坐标系
- **参考坐标系**：使用其它坐标系来考究自身的情况时使用

#### 2.4.1 用户坐标系
#### 2.4.2 自身坐标系
#### 2.4.3 前驱坐标系
#### 2.4.4 参考坐标系

## 2.5 坐标变换
- 定义
- 线性变换
- 线性变换列表
- transform属性

#### 2.5.1 坐标变换定义
#### 2.5.4 transform属性
注意先后顺序（从左到右）

# 3 颜色、渐变和笔刷
## 3.1 认识RGB和HSL
#### 3.1.1 HSL
- 三个分量分别表示颜色、饱和度和亮度
- 格式：hsl(h, s%, l%)
- 取值范围：
  - h: `0 ~ 359`
  - s, l: `0 ~ 100`

#### 3.1.3 透明度
- `rgba(r, g, b, a)`和`hsla(h, s%, l%, a)`表示带透明度的颜色 
- `opacity`属性表示元素的透明度
- `a`和`opacity`的取值范围：`0 ~ 1`

## 3.2 线性渐变和径向渐变
#### 3.2.1 线性渐变
- `<linearGradient>`和`<stop>`
- 定义方向
- 关键点位置及颜色
- `gradientUnits`

```xml
<svg xmlns="http://www.w3.org/2000/svg">
  <defs>
    <linearGradient
      id="grad1"
      gradientUnits="objectBoundingBox"
      x1="0" y1="0"
      x2="1" y2="1">
      <stop offset="0" stop-color="#1497FC" />
      <stop offset="0.5" stop-color="#A469BE" />
      <stop offset="1" stop-color="#FF8C00" />
    </linearGradient>
  </defs>
  <rect 
    x="100" y="100"
    width="200" height="150"
    fill="url(#grad1)"/>
</svg>
```

**`<linearGradient>`属性**：
- `gradientUnits`：
  - `objectBoundingBox`: 默认值，使用对象确定相对位置的矢量点（相对值）
  - `userSpaceOnUse`: 使用视图框以确定相对位置的矢量点（实际值）
- `x1、y1`：渐变向量的起点（默认0％）
- `x2、y2`：渐变向量的终点（默认100％）
- `gradientTransform`： 适用于渐变的变换
- `spreadMethod`
  - `pad`
  - `reflect`
  - `repeat`
- `xlink:href`：引用到另一个渐变，其属性值作为默认值。递归

## 3.2.2 径向渐变
- `<radialGradient>`和`<stop>`
- 定义方向
- 关键点位置及颜色
- `gradientUnits`
- 焦点位置

```xml
<svg xmlns="http://www.w3.org/2000/svg">
  <defs>
    <radialGradient
      id="grad2"
      gradientUnits="objectBoundingBox"
      cx="0.5" cy="0.5"
      r="0.5"
      fx="0.6" fy="0.3">
      <stop offset="0" stop-color="#1497FC" />
      <stop offset="0.5" stop-color="#A469BE" />
      <stop offset="1" stop-color="#FF8C00" />
    </radialGradient>
  </defs>
  <rect 
    x="100" y="100"
    width="200" height="150"
    fill="url(#grad2)"/>
</svg>
```

**`<radialGradient>`属性**：  
同`linearGradient`的属性：
`gradientUnits`、`gradientTransform`、`spreadMethod`、`xlink:href`
- `cx、cy`：渐变的中心点（默认值为50%）
- `r`：渐变的半径
- `fx、fy`：渐变的焦点（默认值为0％）



## 3.3 使用图案（笔刷）
参考：[如何使用SVG图案(w3cplus)](https://www.w3cplus.com/svg/svg-pattern-element.html)

一般用于SVG图形对象的填充`fill`或描边`stroke` (这个图形可以是一个SVG元素，也可以是位图图像，通过<pattern>元素在x轴或y轴方向以固定的间隔平铺著作权归作者所有)

- 绘制纹理
- `<pattern>`标签
- `patternUnits`和`patternContentUnits`属性

```xml
<svg xmlns="http://www.w3.org/2000/svg">
  <defs>
    <pattern
      id="p1"
      x="0" y="0"
      width="0.2" height="0.2">
      <circle
        cx="10" cy="10" r="5"
        fill="red">
      </circle>
      <polygon
        points="30 10 60 50 0 50"
        fill="green">
      </polygon>
    </pattern>
  </defs>
  <rect
    x="100" y="100"
    width="400" height="300"
    fill="url(#p1)"
    stroke="blue">
  </rect>
</svg>
```
**pattern属性**：
- `patternUnits`：
  - `userSpaceOnUse`：百分比单位
  - `objectBoundingBox`：实际值单位
- `patternContentUnits`：控制pattern内部元素的单位，含义同`patternUnits`
  - `userSpaceOnUse`：
  - `objectBoundingBox`：

# 4 path
## 4.1 Path概述
[参考](https://developer.mozilla.org/zh-CN/docs/Web/SVG/Tutorial/Paths)

- 强大的绘图工具
- 由命令及参数组组成的字符串，如：  
  `<path d="M0,0L10,20C30-10,40,20,100,100" stroke="red">`

**`L10,20`**:
- `L`：命令
- `10,20`：参数，参数之间可以用空格或逗号隔开（下一个数值是负数可不用分隔符）

**Path命令汇总**：
命令 | 含义
--- | ---
M/m (x,y)+ | 移动当前位置
L/l (x,y)+ | 从当前位置绘制线段到指定位置
H/h (x)+ | 从当前位置绘制水平线到达指定的x坐标
V/v (y)+ | 从当前位置绘制竖直线到达指定的y坐标
Z/z | 闭合当前路径
Q/q (x1,y1,x,y)+ | 从当前位置绘制二次贝塞尔曲线到指定位置
T/t (x,y)+ | 从当前位置光滑绘制二次贝塞尔曲线到指定位置
C/c (x1,y1,x2,y2,x,y)+ | 从当前位置绘制三次贝塞尔曲线到指定位置
S/s (x2,y2,x,y)+ | 从当前位置光滑绘制三次贝塞尔曲线到指定位置
A/a (rx,ry,xr,laf,sf,x,y) | 从当前位置绘制弧线到指定位置

**命令基本规律**：
- 区分大小写：大写表示坐标参数为绝对位置，小写则为相对位置
- 最后的参数表示最终要到达的位置
- 上一个命令结束的位置就是下一个命令开始的位置
- 命令可以重复参数表示重复执行同一条命令

## 4.2 移动和直线命令
## 4.3 弧线命令(`A/a`)
参数：`(rx,ry,xr,laf,sf,x,y)`  
实际是取的椭圆的一部分

- `rx/ry`：（radius-x/y）弧线所在椭圆的x、y半轴长
- `xr`：（xAxis-rotation）弧线所在椭圆的长轴角度（顺时针方向为正）
- `laf`：（large-arc-flag）0表示短弧线，1表示长弧线
- `sf`：（sweep-flag）0表示逆时针画弧，1表示顺时针画弧
- `x/y`：终点位置

## 4.4 贝塞尔曲线
> `x y`为结束点坐标
### 二次贝塞尔曲线命令(Q/q)
参数：`Q x1 y1 x y`  

**二次贝塞尔光滑曲线(T/t)**：  
C1是上一段曲线的控制点关于当前曲线起始点的镜像位置


### 三次贝塞尔曲线命令(C/c)
参数：`C x1 y1 x2 y2 x y`

**三次贝塞尔光滑曲线(S/s)**：  
C1是上一段曲线的控制点2关于当前曲线起始点的镜像位置

# 5 SVG文本
## 5.1 `<textPath>`和`<tspan>`创建文本
### `<text>`
**属性**：
> 文字的位置为左边线和基线的交点
- `x/y`：文字的起点位置坐标（`y`表示基线位置）
- `dx/dy`：文字的偏移（多个值表示相应方向对应次序开始的文字的偏移位置）

```xml
<!-- 
  文字的起始位置为 1000／1000
  A的位置为 (1000 + 100) / (1000 + 100)
  B的位置为 (1000 + 100 + 200) / (1000 + 100 + 200)
  C的位置为 (1000 + 100 + 200 + 300) / (1000 + 100 + 200 + 300)
-->
<svg>
  <text 
    style="font-size: 50px;"
    x="1000" y="1000" 
    dx="100 200 300" dy="100 200 300">ABC</text>
</svg>
```

### `<tspan>`

```xml
<svg>
  <text dy="200 0 200">
    <!--
      tspan的dx/dy会传递下去
      tspan的dx/dy会覆盖text的dx/dy，后面的tspan会覆盖前面的
    -->
    <tspan 
      fill="red"
      dy="100 100 100">AB</tspan>
    <tspan 
      stroke="green" 
      stroke-width="2" 
      fill="none">CDE</tspan>
  </text>
</svg>
```
## 5.2 垂直居中问题
- `text-anchor`：水平居中属性
- `dominant-baseline`：垂直居中属性
- 模拟垂直居中

### `text-anchor`
可选值：
- `start`
- `middle`
- `end`

### `dominant-baseline`
可选值：
- `auto`
- `use-script`
- `no-change`
- `reset-size`
- `ideographic`
- `alphabetic`
- `hanging`
- `mathematical`
- `central`
- `middle`
- `text-after-edge`
- `text-before-edge`
- `text-top`
- `text-bottom`



### 模拟居中（使用`dy`模拟垂直居中）
假设文字高度为`h`,上边缘为`by`,下边缘为`by + h`, 基线位置为`y`
- `top`: 设置dy=`y - by`
- `middle`: 设置dy=`y - (by + h/2)`
- `bottom`: 设置dy=`y - (by + h)`

## 5.3 `<textPath>`让文本在指定路径上排列

```xml
<svg>
  <path 
    id="path1"
    d="M100 200 Q200 100 300 200 T500 200"
    stroke="rgb(0, 255, 0)"
    fill="none"/>
  <text
    style="font-size: 24px;">
    <textPath xlink:href="#path1">
    这个文字先上去，又下来了。Upside down in english!
    </textPath>
  </text>
</svg>
```

属性：
- `x/y`：设置`<text>/<tspan>`的属性（x表示切线方向, y无效果）
- `text-anchor`：文本长度的中间位置的对齐(`start`, `middle`, `end`)
- `startOffset`：文本在路径上的起点位置(%值，以路径为基础)
- `dx/dy`： 设置`<text>/<tspan>`的属性, 切线(`dx`)和法线(`dy`)方向的偏移

### `<textPath>`路径文本：脚本控制
- `setAttributeNS()`方法设置`xlink:href`属性

```js
const SVG_NS = 'http://www.w3.org/2000/svg'
const XLINK_NS = 'http://www.w3.org/2000/xlink'
```

## 5.4 `<animate>`插入超链接
- 可以加到任意的图形上
- `xlink:href`指定链接地址
- `xlink:title`指定链接提示
- `target`指定打开目标

# 6 图形的引用、裁切和蒙版
## 6.1 `<use>`标签创建图形引用
```xml
<svg 
     width="100%" 
     height="100%"
     viewBox="-400 -300 800 600"
     preserveAspectRatio="xMidYMid slice">
     <!-- 定义 -->
     <defs>
      <polygon 
        id="star"
        points="0 -10 2 -2 10 0 2 2 0 10 -2 2 -10 0 -2 -2"
        fill="white">
      </polygon>
     </defs>
     <!-- 分组 -->
     <g id="star-group">
      <!-- 使用定义 -->
      <use xlink:href="#star"></use>
     </g>
</svg>
```

## 6.2 `<clipPath>`标签裁切图形
保留重叠的区域`clip-path="url(#clip-id)"`
```xml
```

## 6.3 `<mask>`标签创建蒙版
通过亮度来决定被它遮罩的地方显示得多少（白色完全显示，黑色不显示, 其它颜色则是半透明）
```xml
<g id="moon-group">
  <mask id="moon-mask">
    <circle cx="-250" cy="-150" r="100" fill="white"></circle>
    <circle cx="-200" cy="-200" r="100" fill="black"></circle>
  </mask>
  <circle 
    cx="-250" 
    cy="-150" 
    r="100" 
    fill="yellow"
    mask="url(#moon-mask)"></circle>
</g>
```

# 7 SVG动画
## 7.1 动画原理
## 7.2 SMIL for SVG
> 参考：[张鑫旭SMIL animation动画详解](http://www.zhangxinxu.com/wordpress/2014/08/so-powerful-svg-smil-animation/)

### 动画标签
`<animate>`、`<animateTransform>`、`<animateMotion>`

### 动画元素、属性定位以及动画参数设置
- `attributeName`、`attributeType`
- `from`、`to`、`dur`、`repeatCount`、`fill`...
- `calcMode`...

### 设置动画（定位动画目标）
- 定位  
`<animate xlink:href="url(#rect1)"></animate>`
- 被包含在目标元素中
```xml
<rect x="0" ...>
  <animate></animate>
</rect>
```

### 基本动画
设置要进行动画的属性以及变化范围、时间长度(动画可叠加)
```xml
<animate 
  xlink:href="url(#rect1)"
  attributeType="XML" <!-- auto / XML / css -->
  attributeName="x"
  from="10"
  to="110"
  dur="3s"
  fill="freeze"> <!-- remove / freeze -->
</animate>
```

```xml
<svg>
  <rect x="100" y="100" width="100" height="100" fill="red">
    <animate
      id="goright"
      attributeType="XML"
      attributeName="x"
      begin="0; goleft.end + 1s"
      from="100"
      to="500"
      dur="3s"
      fill="freeze">
    </animate>
    <animate
      id="goleft"
      attributeType="XML"
      attributeName="x"
      begin="goright.end + 1s"
      from="500"
      to="100"
      dur="3s"
      fill="freeze">
    </animate>
    <animate
      attributeType="XML"
      attributeName="fill"
      from="red"
      to="yellow"
      dur="6s"
      fill="freeze"
      repeatCount="100"> <!-- infinite -->
    </animate>
  </rect>
</svg>
```

### 变换动画
设置要进行动画的属性以及变化范围、时间长度
```xml
<animateTransform 
  xlink:href="url(#rect1)"
  attributeName="transform"
  attributeType="XML"
  type="translate"
  from="0 0"
  to="100 100"
  dur="3s">
</animateTransform>
```
### 轨迹动画
```xml
<!-- rotate="auto"表示沿路径切线方向运动 -->
<animateMotion 
  xlink:href="url(#rect1)"
  path="M0,0h100v100h-100v-100z"
  rotate="auto"
  duration="3s">
  <!--
  或
  <mpath xlink:href="#motion-path"></mpath> 
  -->
</animateMotion>
```

## 7.3 脚本动画
- 核心思想  
  - `requestAnimationFrame(updateFn)`
- 示例  
  - 力导向图
