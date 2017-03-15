# Selection 对象
`Selection`对象一般由`window.getSelection()`或其他方法产生。它代表页面中的文本选区，可能横跨多个元素。

`Selection`对象所对应的是用户所选择的 ranges （区域），俗称拖蓝。默认情况下，该函数只针对一个区域，我们可以这样使用这个函数：
```js
// window可改为document
// IE9+
var selObj = window.getSelection();
var rangeObj  = selObj.getRangeAt(0);
```
调用`Selection`对象的`toString()`方法会返回被选中区域中的纯文本

## `Selection`属性
### <selObj>.anchorNode
(只读) 返回选区开始位置所属的节点（Node）

### <selObj>.anchorOffset
(只读) 返回一个数字，其表示的是选区起点在 anchorNode 中的位置偏移量

**注意**：  
1. 如果 anchorNode 是文字节点，那么返回的就是从该文字节点的第一个字开始，直到被选中的第一个字之间的字数（如果第一个字就被选中，那么偏移量为零）
2. 如果 anchorNode 是一个元素，那么返回的就是在选区第一个节点之前的同级节点总数。(这些节点都是 anchorNode 的子节点)

### <selObj>.focusNode
(只读) 返回该选区终点所在的节点

### <selObj>.focusOffset
(只读) 返回一个数字，其表示的是选区终点在 focusNode 中的位置偏移量

**注意**：  
1. 如果 focusNode 是文字节点，那么选区末尾未被选中的第一个字，在该文字节点中是第几个字（从0开始计），就返回它。
2. 如果 anchorNode 是一个元素，那么返回的就是在选区末尾之后第一个节点之前的同级节点总数。

### <selObj>.isCollapsed
返回一个布尔值，用于判断选区的起始点和终点是否在同一个位置

### <selObj>.rangeCount
返回该选区所包含的连续范围的数量


## `Selection`方法
### <selObj>.getRangeAt()
返回一个包含当前选区内容的区域`range`对象  

**语法**：
```js
range = selObj.getRangeAt(index);
```
**参数**：
- **index**  
指定需要被处理的子集编号（从零开始计数）。如果该数值被错误的赋予了大于或等于 rangeCount 结果的数字，将会产生错误

```js
var ranges = [];

sel = window.getSelection();

for(var i = 0; i < sel.rangeCount; i++) {
 ranges[i] = sel.getRangeAt(i);
}
/* 在 ranges 数组的每一个元素都是一个 range 对象，
 * 对象的内容是当前选区中的一个。 */
```

### <selObj>.collapse()
(IE9+) 收起当前选区到一个点。( 文档不会发生改变。如果选区的内容是可编辑的并且焦点落在上面，则光标会在该处闪烁。 )

**语法**：
```js
selObj.collapse(parentNode, offset);
```
**参数**：
- **parentNode**  
光标落在的目标节点
- **offset**  
落在节点的偏移量

```js
/* 将光标收起到文档body的开头 */
var body = document.getElementsByTagName("body")[0];
window.getSelection().collapse(body,0);
```

### <selObj>.extend()
将选区的焦点移动到一个特定的位置

**语法**：
```js
selObj.collapse(parentNode, offset);
```
**参数**：
- **parentNode**  
光标落在的目标节点
- **offset**  
落在节点的偏移量

### <selObj>.modify()
（非标准）通过简单的文本命令来改变当前选区或光标位置

**语法**：
```js
selObj.modify(alter, direction, granularity)
```
**参数**：
- **alter**  
改变类型。 传入"move"来移动光标位置，或者"extend"来扩展当前选区。
- **direction**  
调整选区的方向。你可以传入"forward"或"backward"来根据选区内容的语言书写方向来调整。或者使用"left"或"right"来指明一个明确的调整方向。  
- **granularity**  
调整的距离颗粒度。可选值有"character"、"word"、"sentence"、"line"、"paragraph"、"lineboundary"、"sentenceboundary"、"paragraphboundary"、"documentboundary"。

**注意**：  
1. Gecko 不支持 "sentence", "paragraph", "sentenceboundary", "paragraphboundary"和"documentboundary". Webkit和Blink 支持

```js
// 选区向拖选方向扩展一个单词的范围
var selection = window.getSelection();
selection.modify("extend", "forward", "word");
```

### <selObj>.collapseToStart()
（非标准）将当前的选区折叠到起始点。

### <selObj>.collapseToEnd()
（非标准）将当前的选区折叠到最末尾的一个点。

### <selObj>.selectAllChildren()
（非标准）将某一指定节点的子节点框入选区

**语法**：
```js
selObj.selectAllChildren(parentNode)
```
**参数**：
- **parentNode**  
该元素的所有子元素均被选择

```js
footer = document.getElementById("footer");
window.getSelection().selectAllChildren(footer);
/* Everything inside the footer is now selected */
```

### <selObj>.addRange()
向选区中添加一个区域

**语法**：
```js
selObj.addRange(range)
```
**参数**：
- **range**  
一个区域（Range）对象（将被增加到选区（Selection）当中）

```js
/* 在一个HTML文档中选中所有加粗的文本。 */

var strongs = document.getElementsByTagName("strong");
var s = window.getSelection();

if(s.rangeCount > 0) s.removeAllRanges();

for(var i = 0; i < strongs.length; i++) {
  var range = document.createRange();
  range.selectNode(strongs[i]);
  s.addRange(range);
}
```

### <selObj>.removeRange()
将一个区域从选区中移除
**语法**：
```js
selObj.removeRange(range)
```
**参数**：
- **range**  
一个区域（Range）对象（将从选区中删除）

```js
/* 通过设计一段js代码，我们可以获得多个区域，
 * 这段代码可以移除除了第一个区域之外的所有区域。
 * （此代码在Chrome中不生效，因为Chrome当中只能
 * 选择一个选区）*/

var s = window.getSelection();
if(s.rangeCount > 1) {
 for(var i = 1; i < s.rangeCount; i++) {
  s.removeRange(s.getRangeAt(i));
 }
}
```

### <selObj>.removeAllRanges()
（非标准）从当前selection对象中移除所有的range对象 (取消所有的选择只 留下anchorNode 和focusNode属性并将其设置为null)

### <selObj>.deleteFromDocument()
（非标准）从页面中删除选区中的内容 
### <selObj>.containsNode()
（非标准）判断指定的节点是否包含在Selection中(是否被选中)

**语法**：
```js
selObj.containsNode(aNode,aPartlyContained)
```
**参数**：
- **aNode**  
用于判断是否包含在Selection中的那个节点
- **aPartlyContained**  
当此参数为true时, 当selection包含节点aNode的一部分或全部时，containsNode()返回true。
当此参数为false时, 只有当selection完全包含节点aNode时，containsNode() 才返回true。

```js
/* 检查body中是否有节点被选中 */
 console.log(window.getSelection().containsNode(document.body, true));
```

# Range对象
`Range`表示包含节点和部分文本节点的文档片段  

**创建或获取Range对象**：
1. `var range = document.createRange();`
2. `var range = window.getSelection().getRangeAt(0);`
3. `var range = new Range();` IE不支持

## `Range`属性
### <Range>.collapsed
（IE9+）(只读) 返回一个用于判断 Range 起始位置和终止位置是否相同的布尔值。（一个折叠的的Range 是空的，不包含内容，指标是Dom树中的一个点。）

### <Range>.commonAncestorContainer
（IE9+）(只读) 返回包含 startContainer 和 endContainer 的最深的节点

### <Range>.startContainer  
（IE9+）(只读) 返回包含 Range 开始的节点

### <Range>.endContainer 
（IE9+）(只读) 返回包含 Range 终点的节点

### <Range>.startOffset
（IE9+）(只读) 返回 startContainer 中表示 Range 起始位置的数字

**注意**：  
1. 如果 startContainer 是一个文本（Text）、注释（Comment）或者CDATA区块（CDATASection）节点那么返回的偏移量是从 startContainer 开始到Range的边界点的距离。对于其他的节点类型， startOffset 返回 startContainer 到边界点的子元素数量。

### <Range>.endOffset 
返回 endContainer 中表示Range终点位置的数字

## `Range`方法
### <Range>.toString()
把Range内容作为字符串返回

### `定位方法`
### <Range>.setStart()
（IE9+）设置 Range 的起点

**语法**： 
```js
range.setStart(startNode, startOffset);
```
**参数**：
- **startNode**  
用于设定 Range的起始位置
- **startOffset**  
从startNode的开始位置算起的偏移量（必须为不小于0的整数）

**注意**：
1. 如果起始节点类型是 Text， Comment, or CDATASection之一, 那么 startOffset指的是从起始节点算起字符的偏移量。 对于其他 Node 类型节点， startOffset 是指从起始结点开始算起子节点的偏移量。

```js
var range = document.createRange();
var startNode = document.getElementsByTagName("p").item(2);
var startOffset = 0;
range.setStart(startNode,startOffset);
```

### <Range>.setEnd()
（IE9+）设置 Range 的终点

**语法**： 
```js
range.setStart(endNode, endOffset);
```
**参数**：
- **endNode**  
用于设定 Range的起始位置
- **endOffset**  
从endNode的开始位置算起的偏移量（必须为不小于0的整数）

```js
var range = document.createRange();
var endNode = document.getElementsByTagName("p").item(3);
var endOffset = endNode.childNodes.length;
range.setEnd(endNode, endOffset);
```

### <Range>.setStartBefore()
（IE9+）以其它节点为基准，设置 Range 的起点

**语法**：
```js
range.setStartBefore(referenceNode);
```
**参数**：
- **referenceNode**  
设置Range的起点位于该节点前

```js
var range = document.createRange();
var referenceNode = document.getElementsByTagName("div").item(0);

range.setStartBefore(referenceNode);
```

### <Range>.setStartAfter()
用法同上
### <Range>.setEndBefore()
用法同上
### <Range>.setEndAfter()
用法同上

### <Range>.selectNode(node)
（IE9+）设定一个包含节点和节点内容的 Range 

### <Range>.selectNodeContents(node)
设定一个包含某个节点内容的 Range 

### <Range>.collapse(Boolean)
向指定端点折叠该 Range。(参数默认为false，表示折叠到末尾。true表示折叠到起始位置)

### `编辑方法`
### <Range>.cloneContents()
（IE9+）返回 Range 当中节点的文档片段（DocumentFragment）

```js
var range = document.createRange();
range.selectNode(document.getElementsByTagName("div").item(0));
var documentFragment = range.cloneContents();
document.body.appendChild(documentFragment);
```
### <Range>.deleteContents()
（IE9+）从文档（Document）中移除 Range 中的内容

```js
var range = document.createRange();
range.selectNode(document.getElementsByTagName("div").item(0));
range.deleteContents();
```

### <Range>.extractContents()
（IE9+）把 Range 的内容从文档树移动到文档片段（DocumentFragment）中

```js
var range = document.createRange();
range.selectNode(document.getElementsByTagName("div").item(0));
var documentFragment = range.extractContents();
document.body.appendChild(documentFragment);
```

### <Range>.insertNode(newNode)
（IE9+）在 Range 的起点处插入节点

```js
var range = document.createRange();
newNode = document.createElement("p");
newNode.appendChild(document.createTextNode("New Node Inserted Here"));
range.selectNode(document.getElementsByTagName("div").item(0));
range.insertNode(newNode);
```

### <Range>.surroundContents(newNode)
（IE9+）将 Range 的内容移动到一个新的节点中（并将这个新节点放到指定区域）

**注意**：
1. 这个方法与newNode.appendChild(range.extractContents()); range.insertNode(newNode)很相近. 选定后, range拥有了节点标签.
2. 也有例外情况, 如果选定的Range区域包含仅有一个节点标签的Text. 那标签将不会自动成对生成，操作将失败

```js
var range = document.createRange();
var newNode = document.createElement("p");

range.selectNode(document.getElementsByTagName("div").item(0));
range.surroundContents(newNode);
```

### `其他方法`
### <Range>.compareBoundaryPoints()
（IE9+）比较两个 Range 的端点

**语法**：
```js
compare = range.compareBoundaryPoints(how, sourceRange);
```
**参数c**：
- **how**  
  - **Range.END_TO_END**  
    比较<Range>和sourceRange的终点
  - **Range.START_TO_START**  
    比较<Range>和sourceRange的起点
  - **Range.START_TO_END**  
    比较<Range>的终点和sourceRange的起点
  - **Range.END_TO_START**  
    比较<Range>的起点和sourceRange的终点
- **sourceRange**  
要比较的Range对象

```js
var range, sourceRange, compare;
range = document.createRange();
range.selectNode(document.getElementsByTagName("div")[0]);
sourceRange = document.createRange();
sourceRange.selectNode(document.getElementsByTagName("div")[1]);
compare = range.compareBoundaryPoints(Range.START_TO_END, sourceRange);
```

### <Range>.cloneRange()
（IE9+）返回拥有和原 Range 相同端点的克隆 Range 对象

### <Range>.detach()
（IE9+）从使用状态释放 Range，此方法用于改善性能

### <Range>.getBoundingClientRect()
（非标准）返回一个 `ClientRect` 对象，该对象限定了选定的文档对象的内容，该方法返回了一个矩形，这个矩形包围了该文档对象中所有元素的边界矩形集合

### <Range>.getClientRects()
（非标准）

**参考**：
- https://developer.mozilla.org/zh-CN/docs/Web/API/Selection
- https://developer.mozilla.org/zh-CN/docs/Web/API/Range