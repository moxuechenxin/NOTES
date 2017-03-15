## 7阶层叠水平（层叠顺序）
属性|层叠顺序
--|--
正`z-index` | 7
`z-index:auto`或看成`z-index:0`不依赖`z-index`的层叠上下文 | 6
`inline/inline-block`水平盒子 | 5
`float`浮动盒子|4
`block`块状水平盒子|3
负`z-index`|2
层叠上下文`background/border` | 1

## 创建层叠上下文：
- 页面根元素
- `z-index`不为`auto`（为0也能创建层叠上下文且IE7为auto时也创建层叠上下文）

## 层叠上下文内的层叠水平
- 同级后来居上（普通元素间也遵循）
- 谁大谁上
- 祖先优先原则（`z-index`都不为`auto`的情况）；
- z-index为负时，只在层叠上下文`background`和`border`之上，譬如：父级元素`relative`（`z-index`默认为`auto`，未创建层叠上下文），子元素`absolute`且`z-index:-1`，则子元素居于父元素`background`和`border`之下，往上查找层叠上下文元素直至根元素

**z-index**:
- 支持`animation`动画；
- 只有`position`值不为`static`时有效；

## 其他创建层叠上下文的方法（ 层叠顺序为z-index:auto级别）：
- `z-index`值不为`auto`的`flex`项（父元素`display:flex | inline-flex`）
- 元素的`opacity`值不为1
- 元素的`transform`值不为`none`
- `position:fixed`声明（仅webkit、blink内核浏览器支持）
- 元素的`filter`值不是`none`
- 元素的`mix-blend-mode`值不为`normal`
- 元素的`isolation`值不是`none`
- `will-change`指定的属性值为上面任意一个
- 元素的`-webkit-overflow-scrolling`设为`touch`

**参考**：
- http://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/