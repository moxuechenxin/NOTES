## 概述
渐变可以应用于任何使用背景图片的地方。理论上可在任何使用url() 值的地方采用，比如最常见的background-image、list-style-type以及前面介绍的CSS3的图像边框属性border-image。**但直到目前为止，仅在背景图片中得到最完美的支持**

## 线性渐变（linear-gradient , IE10+）
### 语法和参数
#### 旧标准语法
```
linear-gradient([<angle> | <side-or-corner>,]?<stop>,<stop>[,<stop>]*)
```
**需要加厂商前缀**：
`-webkit-`,`-moz-`,`-ms-`,`-o-`

#### W3C标准语法
```
linear-gradient([<angle> | to <side-or-corner>,]?<color-stop>,<color-stop>[,<color-stop>]*) 
```

**参数**：

- **angle | side-or-corner**：（默认：从上到下）
    - **角度**：`0deg`,`45deg`...
    **旧标准**：`0deg`表示从左到右。以逆时针方向计算角度  
    **新标准**：`0deg`表示从下到上。以顺时针方向计算角度。新旧标准的换算关系：90 - new = old)
    - **边或角**：`left`,`right`,`top`,`bottom`,`left top`,`right top`....
    **旧标准**：表示起点边或角
    **新标准**：位置前加`to`，表示终点边或角
    
- **color-stop**：`<color> [ <percentage> | <length> ]`：颜色值+空格+百分比或长度值


```css
/* 从左至右：从红色向绿色渐变 */
.grad {
  /* Safari 5.1 - 6.0 */
  background: -webkit-linear-gradient(left,red,green);
  /* Opera 11.1 - 12.0 */
  background: -o-linear-gradient(left,red,green);
  /* Firefox 3.6 - 15 */
  background: -moz-linear-gradient(left,red,green);
  /* 标准的语法 */
  background: linear-gradient(to right,red,green); 
}

/* 从左至右：从完全透明的红色渐变到完全不透明的红色 */
.grad {
  /* Safari 5.1 - 6.0 */
  background: -webkit-linear-gradient(left,rgba(255,0,0,0),rgba(255,0,0,1));
  /* Opera 11.1 - 12.0 */
  background: -o-linear-gradient(left,rgba(255,0,0,0),rgba(255,0,0,1));
  /* Firefox 3.6 - 15 */
  background: -moz-linear-gradient(left,rgba(255,0,0,0),rgba(255,0,0,1));
  /* 标准的语法 */
  background: linear-gradient(to right,rgba(255,0,0,0),rgba(255,0,0,1)); 
}

/* 从左至右：从红色向绿色渐变 */
.grad {
  /* Safari 5.1 - 6.0 */
  background: -webkit-linear-gradient(0deg,red,green);
  /* Opera 11.1 - 12.0 */
  background: -o-linear-gradient(0deg,red,green);
  /* Firefox 3.6 - 15 */
  background: -moz-linear-gradient(0deg,red,green);
  /* 标准的语法 */
  background: linear-gradient(90deg,red,green); 
}

/* 从左至右：0~10%的区间内是红色，10%~50%区间从红色渐变到蓝色，50%~100%区间是蓝色  */
.grad {
  /* Safari 5.1 - 6.0 */
  background: -webkit-linear-gradient(0deg,red 10%, blue 50%);
  /* Opera 11.1 - 12.0 */
  background: -o-linear-gradient(0deg,red 10%, blue 50%);
  /* Firefox 3.6 - 15 */
  background: -moz-linear-gradient(0deg,red 10%, blue 50%);
  /* 标准的语法 */
  background: linear-gradient(90deg,red 10%, blue 50%); 
}
```

## 重复的线性渐变（repeating-linear-gradient , IE10+）
```css
.grad {
  /* Safari 5.1 - 6.0 */
  background: -webkit-repeating-linear-gradient(left,red, yellow 10%, green 20%red, yellow 10%, green 20%);
  /* Opera 11.1 - 12.0 */
  background: -o-repeating-linear-gradient(left,red, yellow 10%, green 20%red, yellow 10%, green 20%);
  /* Firefox 3.6 - 15 */
  background: -moz-repeating-linear-gradient(left,red, yellow 10%, green 20%red, yellow 10%, green 20%);
  /* 标准的语法 */
  background: repeating-linear-gradient(to right,red, yellow 10%, green 20%red, yellow 10%, green 20%);
}
```

## 径向渐变 （radial-gradient, IE10+）
### 语法和参数
#### 旧标准语法
```
radial-gradient([<position> | <angle>,]?[<shape> | <size>,]?<color-stop>,<color-stop>[,<color-stop>]*);	
```
**需要加厂商前缀**：
`-webkit-`,`-moz-`,`-ms-`,`-o-`

**参数**：

#### W3C标准语法
```
radial-gradient([ [<shape> | <size>] [at <position>]?,| at <position>,]?<color-stop>,<color-stop>[,<color-stop>]*);
```
**参数**：

- **position**：主要用来定义径向渐变的圆心位置。默认为`center`
  **可选值种类**：
    - `center`：默认值。设置中间为径向渐变圆心的横坐标值或纵坐标
    - `<length>`： 用长度值指定径向渐变圆心的横坐标或纵坐标。(可以为负值)。
    - `<percentage>`：用百分比指定径向渐变圆心的横坐标或纵坐标。(可以为负值)。
    - `left`,`right`：设置左(右)边为径向渐变圆心的横坐标值。
    - `top`,`bottom`：设置顶(底)部为径向渐变圆心的纵标值。

- **shape**：定义径向渐变的形状
  **可选值**：
    - `ellipse`：默认值。椭圆形
    - `circle`：圆形。

- **size**：径向渐变的结束形状大小
  **当指定了`shape`,可选值**：
    - `farthest-corner`：默认值。指定径向渐变的半径长度为从圆心到离圆心最远的角
    - `farthest-side`：指定径向渐变的半径长度为从圆心到离圆心最远的边
    - `closest-corner`：指定径向渐变的半径长度为从圆心到离圆心最近的角
    - `closest-side`：指定径向渐变的半径长度为从圆心到离圆心最近的边

  **未指定`shape`**：可设置为`<radius-x> <radius-y>`，分别为渐变的水平、竖直半径（不能为负），可选值种类：
    - `<legnth>`：用长度值指定半径
    - `<percentage>`：相对于径向渐变容器的尺寸
    当两个方向半径相同则形状为圆形，否则为椭圆

- **color-stop**：`<color> [ <percentage> | <length> ]`：颜色值+空格+百分比或长度值。其中0%表示渐变线的起始点，100%表示渐变线的与渐变容器相交结束的位置。而且其颜色停止可以定义一个负值。

```css
.grad {
  /* Safari 5.1 - 6.0 */
  background: -webkit-radial-gradient(circle,red 5%, green 15%, blue 60%);
  /* Opera 11.6 - 12.0 */
  background: -o-radial-gradient(circle,red 5%, green 15%, blue 60%); 
  /* Firefox 3.6 - 15 */
  background: -moz-radial-gradient(circle,red 5%, green 15%, blue 60%); 
  /* 标准的语法 */
  background: radial-gradient(circle,red 5%, green 15%, blue 60%); 
}

.grad {
  /* Safari 5.1 - 6.0 */
  background: -webkit-radial-gradient(60% 55%, closest-side,red 5%, green 15%, blue 60%);
  /* Opera 11.6 - 12.0 */
  background: -o-radial-gradient(60% 55%, closest-side,red 5%, green 15%, blue 60%); 
  /* Firefox 3.6 - 15 */
  background: -moz-radial-gradient(60% 55%, closest-side,red 5%, green 15%, blue 60%); 
  /* 标准的语法 */
  background: radial-gradient( closest-side at 60% 55% ,red 5%, green 15%, blue 60%); 
}

```

## 重复的径向渐变（repeating-radial-gradient，IE10+）
```css
.grad {
  /* Safari 5.1 - 6.0 */
  background: -webkit-repeating-radial-gradient(red, yellow 10%, green 15%);
  /* Opera 11.6 - 12.0 */
  background: -o-repeating-radial-gradient(red, yellow 10%, green 15%);
  /* Firefox 3.6 - 15 */
  background: -moz-repeating-radial-gradient(red, yellow 10%, green 15%);
  /* 标准的语法 */
  background: repeating-radial-gradient(red, yellow 10%, green 15%);
}
```

**参考**：  
- [菜鸟教程：CSS3 渐变（Gradients）](http://www.runoob.com/css3/css3-gradients.html)
- [w3cplus：再说CSS3渐变——线性渐变](http://www.w3cplus.com/css3/new-css3-linear-gradient.html)
- [w3cplus：再说CSS3渐变——径向渐变](http://www.w3cplus.com/css3/new-css3-radial-gradient.html)
- [深入理解CSS3 gradient斜向线性渐变](http://www.zhangxinxu.com/wordpress/2013/09/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3css3-gradient%E6%96%9C%E5%90%91%E7%BA%BF%E6%80%A7%E6%B8%90%E5%8F%98/)


