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
- `rx`、`ry`：矩形圆角（如果只定义其一，则另一属性值相同）

#### `<circle>` 圆
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
- `points`：点坐标集合（首尾点相连图形闭合）

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
- 如果宽高比和viewport相同，将会拉伸填充满整个viewport
- 如果宽高比和viewport不同，使用`preserveAspectRatio`控制显示行为

**preserveAspectRatio**：  
**语法**：preserveAspectRatio = [defer] <align> [meetOrSlice>]  
**参数**：
- `defer`: 可选，仅在`image`元素上应用`preserveAspectRatio`属性时才使用 
- `align`: 默认值`xMidYMid`，指定`viewbox`在viewport中的对齐方式（除了`none`其余值由两部分组成：X方向的对齐方式、Y方向的对齐方式）

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
meet	| 默认值, 保持宽高比并将viewBox按长宽比中的较小值缩放(类似background-size的contain)
slice |	保持宽高比并将viewBox按长宽比中的较大值缩放(类似background-size的cover)
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
**说明**：viewport的尺寸为`500 * 75`， viewBox为 `0 0 250 25`，X轴方向的比值为 500 / 250 = 2, Y轴为 75 / 25 = 3, 则`meetOrSlic`为`meet`时，viewBox长宽都缩放2倍；为`slice`时，缩放3倍；为`none`时，宽度缩放2倍，长度缩放3倍（效果就是viewBox相关方向的尺寸也跟着缩放）

## SVG中的图形分组
## 坐标