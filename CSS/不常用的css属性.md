### 概览
- **initial 关键字**
- **text-transform**：控制文本大小写
- **writing-mode**：书写方向
- **direction**：文本流方向
- **unicode-bidi**：配合`direction`使用
- **table-layout**：用来显示表格单元格、行、列的算法规则
- **tab-size**：规定制表符（tab）字符的空格长度
- **backface-visibility**：定义当元素不面向屏幕时是否可见
- **font-variant**：设置小型大写字母的字体显示文本

### [initial关键字](http://www.css88.com/book/css/values/textual/initial.htm)
（IE不支持）`initial`属于`CSS-wide`关键字，这表示所有的属性都可以接受该值。如果你想重置某个属性为UA默认设置，那么就可以使用该值

### [unset关键字](http://www.css88.com/book/css/values/textual/unset.htm)
（IE不支持）一个属性定义了`unset`值，如果该属性是默认继承属性，该值等同于`inherit`，如果该属性是非继承属性，该值等同于`initial`。`unset`属于`CSS-wide`关键字，这表示所有的属性都可以接受该值

### [text-transform](http://www.w3school.com.cn/cssref/pr_text_text-transform.asp)
控制文本的大小写  

**继承性**：yes  
**可选值**：
值	| 描述
--|--
none  | 	**默认值**。定义带有小写字母和大写字母的标准的文本。
capitalize  |	文本中的每个单词以大写字母开头。
uppercase	|  定义仅有大写字母。
lowercase   |	定义无大写字母，仅有小写字母。
inherit	| 规定应该从父元素继承 text-transform 属性的值。

**注意**：
- 不同的用户代理可能会用不同的方法来确定单词从哪里开始，相应地确定哪些字母要大写。例如，文本 "w3-school" 可以用两种方式显示："W3-school" 和 "W3-School"。CSS 并没有规定哪一种是正确的，所以这两种都是可以的。


### writing-mode
设置对象的内容块固有的书写方向

**继承性**：yes  
**可选值**：
值 | 描述
--|--
normal | **默认值**
horizontal-tb | 水平方向自上而下的书写方式。即 left-right-top-bottom（类似IE私有值lr-tb）
vertical-rl | 垂直方向自右而左的书写方式。即 top-bottom-right-left（类似IE私有值tb-rl）
vertical-lr | 垂直方向自左而右的书写方式。即 top-bottom-left-right
lr-tb | 左-右，上-下。对象中的内容在水平方向上从左向右流入，后一行在前一行的下面。<br>所有的字形都是竖直向上的。这种布局是罗马语系使用的（IE）
tb-rl | 上-下，右-左。对象中的内容在垂直方向上从上向下流入，自右向左。后一竖行在前一竖行的左面。<br>全角字符是竖直向上的，半角字符如拉丁字母或片假名顺时针旋转90度。这种布局是东亚语系通常使用的（IE）

**注意**
- chrome和safari下，为了兼容性，写作`-webkit-writing-mode`

**参考**：
- http://www.css88.com/book/css/properties/writing-modes/writing-mode.htm
- http://www.zhangxinxu.com/wordpress/2016/04/css-writing-mode/

### direction
设置文本流的方向

**继承性**：yes  
**可选值**：
值 | 描述
--|--
ltr | **默认值**。文本流从左到右
rtl | 文本流从右到左（需同`unicode-bidi`配合使用）

**注意**：
- 当指定一个内联元素的 `unicode-bidi` 属性为normal时， `direction` 属性设置不影响bidi重排序，即其设置不生效

### [unicode-bidi](http://www.css88.com/book/css/properties/writing-modes/unicode-bidi.htm)
用于同一个页面里存在从不同方向读进的文本显示。需与`direction`属性一起使用

**继承性**：no  
**可选值**：
值 | 描述
--|--
normal | **默认值**。对象不打开附加的嵌入层，对于内联元素，隐式重排序跨对象边界进行工作。
embed | 对象打开附加的嵌入层，`direction` 属性的值指定嵌入层，在对象内部进行隐式重排序。
bidi-override | 严格按照`direction` 属性的值重排序。忽略隐式双向运算规则
isolate (CSS3) | 
isolate-override (CSS3)  | 
plaintext (CSS3) |


### [table-layout](http://www.w3school.com.cn/cssref/pr_tab_table-layout.asp)
用来显示表格单元格、行、列的算法规则

**继承性**：yes  
**可选值**：
值 | 描述
--|--
auto | **默认值**。列的宽度是由列单元格中没有折行的最宽的内容设定的。<br>（此算法有时会较慢，这是由于它需要在确定最终的布局之前访问表格中所有的内容。）
fixed | 水平布局仅取决于表格宽度、列宽度、表格边框宽度、单元格间距，而与单元格的内容无关。<br>（通过使用固定表格布局，用户代理在接收到第一行后就可以显示表格。
）


### tab-size
规定制表符（tab）字符的空格长度。（在 HTML 中，制表符（tab）字符通常显示为一个单一的空格字符，除了一些元素，比如 `<textarea>` 和 `<pre>`，tab-size 属性的结果只对这些元素有效）  

**兼容性**：
目前只有 Chrome 支持 tab-size 属性。  
Firefox 支持另一个可替代该属性的属性，即 `-moz-tab-size` 属性。  
Opera 支持另一个可替代该属性的属性，即 `-o-tab-size` 属性。

**继承性**：yes  
**可选值**：
值 | 描述
--|--
number	| 默认值是 8。规定每个制表符（tab）字符要显示的空格字符的数量。
length	| 规定制表符（tab）字符的长度。几乎所有的主流浏览器都不支持该属性值。	
initial	| 设置该属性为它的默认值。
inherit	| 从父元素继承该属性。

### [backface-visibility](http://www.w3school.com.cn/cssref/pr_backface-visibility.asp)
（IE10+）定义当元素不面向屏幕时是否可见（例如：绕x轴或y轴旋转180deg）  

**厂商前缀**：   
`-webkit-`,`-moz-`,`-ms-`

**可选值**：
值 | 描述
--|--
visible | **默认**。背面是可见的。
hidden | 背面是不可见的。

### [font-variant](http://www.w3school.com.cn/cssref/pr_font_font-variant.asp)
设置小型大写字母的字体显示文本，这意味着所有的小写字母均会被转换为大写，但是所有使用小型大写字体的字母与其余文本相比，其字体尺寸更小

**继承性**：yes  
**可选值**：
值 | 描述
--|--
normal | 默认值。浏览器会显示一个标准的字体。
small-caps | 浏览器会显示小型大写字母的字体。
inherit | 规定应该从父元素继承 `font-variant` 属性的值 (IE9+)
