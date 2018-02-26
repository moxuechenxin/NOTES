### rem和em
- `rem`是相对于根元素(`<html>`) font-size的单位
（1rem = html元素的`font-size`大小）
- `em`是相对于当前元素font-size的单位（1em = 当前元素的font-size）

### vh和vw ( IE9+ )
- 1`vw`等于1%的视口宽度
- 1`vh`等于1%的视口高度

### vmin和vmax ( IE9通过不标准的名称 vm 来支持 vmin,不支持vmax)
- `vmin`和`vmax`表示视口高度和宽度两者的最小或者最大值（也是1%的换算关系）

### ex和ch (IE9+)
- `ch`通常被定义为数字0的宽度
- `ex`定义为当前字体的小写x的高度或者1/2的em。很多时候，它是字体的中间标志

**参考**：  
- [CSS：7个你可能不认识的单位](https://github.com/dwqs/blog/issues/31)
- [菜鸟教程：CSS单位](http://www.runoob.com/cssref/css-units.html)