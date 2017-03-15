## 1. screen对象
#### 1.1 获取屏幕的高宽

```js
screen.width         //屏幕的宽
screen.height        //屏幕的高
screen.availWidth    //屏幕可用宽度   屏幕的像素高度 - 系统部件高度
screen.availHeight   //屏幕可用高度   屏幕的像素宽度 - 系统部件宽度
```
## 2. window对象
#### 2.1 获得窗口位置及大小

```js
window.screenTop            //窗口顶部距屏幕顶部的距离

window.screenLeft           //窗口左侧距屏幕左侧的距离

window.innerWidth           // (IE9+)窗口中可视区域(viewpoint)的宽度
alert(window.innerWidth);   //chrome 1366 ff 1366 ie 1366

window.innerHeight         // (IE9+)窗口中可视区域(viewpoint)的高度该值与浏览器是否显示菜单栏等因素有关
alert(window.innerHeight);  //chrome 643  ff 657  ie 673

window.outerWidth          // (IE9+)浏览器窗口本身的宽度(可视区域宽度+浏览器边框宽度)
alert(window.outerWidth);   //chrome 1366 ff 1382 ie 1382  
                        //说明chrome在最大化时浏览器窗口没有边框宽度,非最大化时有8px边框
                        //ff和ie上下左右有8px的边框宽度

window.outerHeight        // (IE9+)浏览器窗口本身的高度
alert(window.outerHeight);  //chrome 728 ff 744 ie 744
```
**window.pageXOffset（window.scrollX）**：  设置或返回当前页面相对于窗口显示区左上角的 X 位置(IE不支持)    
**window.pageYOffset（window.scrollY）**：  设置或返回当前页面相对于窗口显示区左上角的 Y 位置(IE不支持) 

## 3. element对象
**offsetParent**：  
\- 只读属性，返回一个指向最近的（closest，指包含层级上的最近）包含该元素的定位元素    
\- 如果没有定位的元素，则 offsetParent 为最近的 table 元素对象或根元素（标准模式下为 html；quirks 模式下为 body）  
\- 当元素的 style.display 设置为 "none" 时，offsetParent 返回 null    
\- 在 Webkit 中，如果元素为隐藏的（该元素或其祖先元素的 style.display 为 "none"），或者该元素的 style.position 被设为 "fixed"(依旧可以读取其offsetLeft和offsetTop属性)，则该属性返回 null。  
\- 在 IE 9 中，如果该元素的 style.position 被设置为 "fixed"，则该属性返回 null。（display:none 无影响。）

### 3.1 当不出现滚动条时

```html
<style>
body{margin:0;}
#demo{
    width:100px;
    height:100px;
    padding:10px;
    border:20px;
    margin:30px;
    background-color:red;
}
</style>
<div id="demo">123456789 123456789</div>
```
**clientWidth**：  在页面上返回内容的可视宽度（不包括边框，边距或滚动条）  
**clientHeight**： 在页面上返回内容的可视高度（不包括边框，边距或滚动条）  

```js
// clientWidth = 2*padding + content-width - scrollbarWidth     
console.log(document.getElementById('demo').clientWidth); //120 此时scrollbarWidth=0

// clientHeight = 2*padding + content-height - scrollbarHeight
console.log(document.getElementById('demo').clientHeight); //120 此时scrollbarWidth=0
```
**clientLeft**：  在页面上返回元素左边框(`border-left`)的厚度  
**clientTop**： 在页面上返回元素上边框(`border-top`)的厚度  

```js
console.log(document.getElementById('demo').clientLeft); //1 ,border-left为1px

console.log(document.getElementById('demo').clientTop); // 1 ,border-top为1px
```
**offsetWidth**：返回元素的宽度包括边框和填充，但不包括边距  
**offsetHeight**：返回元素的高度包括边框和填充，但不包括边距

```js
// offsetWidth = 2*border + 2*padding + content-width  
console.log(document.getElementById('demo').offsetWidth)    //160

// offsetHeight = 2*border + 2*padding + content-height  
console.log(document.getElementById('demo').offsetHeight)   //160
```
**offsetLeft**： 获取对象元素边界的左上角顶点相对于offsetParent的左上角顶点的水平直偏移量  
**offsetTop**： 获取对象元素边界的左上角顶点相对于offsetParent的左上角顶点的竖直偏移量

```js
// 此内容有待考证

// IE6~7
offsetLeft = (offsetParent的padding-left) + (当前元素的margin-left)

// IE8~10,chrome
offsetLeft = (offsetParent的margin-left) + (offsetParent的border宽度) + (offsetParent的padding-left) + (当前元素的margin-left)

// firefox
offsetLeft = (offsetParent的margin-left) + (offsetParent的padding-left) + (当前元素的margin-left)
```  
### 3.2 当出现滚动条时（或者说是实际尺寸）

```html
<style>
body{
    margin:0;
    padding:20px;
    width:1000px;
    height:500px;
}
</style>
<div id="demo">123456789123456789</div>
```
**scrollWidth**： 返回元素的整个宽度（包括带滚动条的隐蔽的地方）  
**scrollHeight**： 返回整个元素的高度（包括带滚动条的隐蔽的地方）

```js
// scrollWidth = 2*padding + width    
console.log(document.body.scrollWidth)    //1040

// scrollHeight = 2*padding + height   
console.log(document.body.scrollHeight)   //540
```
**scrollTop(可读写)**： 向下滑动滚动块时元素隐藏内容的高度。不设置时默认为0，其值随着滚动块滚动而变化  
**scrollLeft(可读写)**： 向右滑动滚动块时元素隐藏内容的宽度。不设置时默认为0，其值随着滚动块滚动而变化  

**注意**：


## 4. event对象
##### Event 对象代表事件的状态，比如事件在其中发生的元素、键盘按键的状态、鼠标的位置、鼠标按钮的状态。

**event.pageX** : 相对整个页面的坐标，以页面的左上角为坐标原点到鼠标所在点的水平距离（IE9+，低版本IE可用 event.clientX + (document.body.scrollLeft || document.documentElement.scrollLeft)计算出 ）    
**event.pageY** : 相对整个页面的坐标，以页面的左上角为坐标原点到鼠标所在点的垂直距离（IE9+，低版本IE可用 event.clientY + (document.body.scrollTop || document.documentElement.scrollTop)计算出 ）  

**event.clientX** : 相对可视区域的坐标，以浏览器可视区域左上角为坐标原点到鼠标所在点的水平距离    
**event.clientY** : 相对可视区域的坐标，以浏览器可视区域左上角为坐标原点到鼠标所在点的垂直距离  

**event.screenX** : 相对电脑屏幕的坐标，以屏幕左上角为坐标原点到鼠标所在点的水平距离    
**event.screenY** : 相对电脑屏幕的坐标，以屏幕左上角为坐标原点到鼠标所在点的垂直距离  

**event.offsetX** : 相对于自身的坐标，以自身的padding左上角为坐标原点到鼠标所在点的水平距离（IE）    
**event.offsetY** : 相对于自身的坐标，以自身的padding左上角为坐标原点到鼠标所在点的水平距离（IE） 

 

## 5. 相关方法 
#### Element.getBoundingClientRect()
返回元素的尺寸（IE8及以下不能获取到宽高）及其相对于视口左上角的位置,如下：

```
// width/height是最外围的尺寸，即content + padding + border 
{
    bottom : 443,
    height : 29,
    left : 286.625,
    right : 593.625,
    top : 414,
    width : 307
}
```


#### window.scroll(x,y)
滚动到document的某一个位置点 , 也可写作`window.scrollTo(x,y)`

#### Document.elementFromPoint(x,y)
返回当前文档上处于指定坐标位置最顶层的元素（坐标是相对于包含该文档的浏览器窗口的左上角为原点来计算的, 通常 x 和 y 坐标都应为正数）

参考：https://developer.mozilla.org/zh-CN/docs/Web/API/Document/elementFromPoint


## 应用
#### 获取浏览器可视区域大小（缩放比例为1）：
参考：http://www.imooc.com/code/1702

```js
var w,h;
// IE9+
w = window.innerWidth;
h = window.innerHeight;

//IE5~8
w = document.documentElement.clientHeight; // 表示HTML文档所在窗口的当前高度
h = document.documentElement.clientWidth; // 表示HTML文档所在窗口的当前宽度

// 或者
// Document对象的body属性对应HTML文档的<body>标签
w = document.body.clientWidth;
h = document.body.clientHeight;
// 兼容写法
w = document.documentElement.clientHeight || document.body.clientWidth;
h = document.documentElement.clientWidth || document.body.clientHeight;
```

#### 获取页面缩放比例
```js
var scale = (document.body.clientWidth || document.documentElement.clientWidth) / window.innerWidth 
```

#### 获取页面滚动轴位置
参考：https://developer.mozilla.org/zh-CN/docs/Web/API/Window/scrollY

```js
var supportPageOffset = window.pageXOffset !== undefined;
var isCSS1Compat = ((document.compatMode || "") === "CSS1Compat");

var x = supportPageOffset ? window.pageXOffset : isCSS1Compat ? document.documentElement.scrollLeft : document.body.scrollLeft;
var y = supportPageOffset ? window.pageYOffset : isCSS1Compat ? document.documentElement.scrollTop : document.body.scrollTop;
```


**参考**: 
- http://www.cnblogs.com/jesse131/p/5709285.html
- http://www.zhangxinxu.com/wordpress/2011/09/cssom%E8%A7%86%E5%9B%BE%E6%A8%A1%E5%BC%8Fcssom-view-module%E7%9B%B8%E5%85%B3%E6%95%B4%E7%90%86%E4%B8%8E%E4%BB%8B%E7%BB%8D/
