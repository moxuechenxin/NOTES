# history对象
当页面的 URL 改变时，就会生成一条历史记录。在 IE8 及更高版本、Opera、
Firefox、Safari 3 及更高版本以及 Chrome 中，这里所说的改变包括 URL 中 hash 的变
化（因此，设置 location.hash 会在这些浏览器中生成一条新的历史记录）。
### 1. 属性
#### 1.1 length (只读)
\- 保存着历史记录的数量。这个数量
包括所有历史记录，即所有向后和向前的记录。对于加载到窗口、标签页或框架中的第一个页面而言，
history.length 等于 0。
```js
if (history.length == 0){
//这应该是用户打开窗口后的第一个页面
}
```
### 2. 方法
#### 2.1 go()
\- 在用户的历史记录中任意跳转  

**参数(一个参数)**：  
　　**数字**：
负数表示向后跳转（类似于单击浏览器的“后退”按钮），
正数表示向前跳转（类似于单击浏览器的“前进”按钮）
```js
//后退一页
history.go(-1);
//前进一页
history.go(1);
//前进两页
history.go(2);
```
　　**字符串**：
浏览器会跳转到历史记录中包含该字符串的第一个
位置——可能后退，也可能前进，具体要看哪个位置最近。如果历史记录中不包含该字符串，那么这个
方法什么也不做
```js
//跳转到最近的 wrox.com 页面
history.go("wrox.com");
//跳转到最近的 nczonline.net 页面
history.go("nczonline.net");
```
#### 2.2 back()
\- 后退一页，相当于history.go(-1)
#### 2.3 forward()
\- 前进一页，相当于history.go(1)
# 历史状态管理(H5) IE10+
HTML5引进了history.pushState()方法和history.replaceState()方法，它们允许你逐条地添加和修改历史记录条目。这些方法可以协同window.onpopstate事件一起工作。
### 1. 方法
#### 1.1 history.pushState()
\- 执行 pushState() 方法后，新的状态信息就会被加入历史状态栈，而浏览器地址栏也会变成新的
相对 URL。但是，浏览器并不会真的向服务器发送请求，即使状态改变之后查询 location.href 也会
返回与地址栏中相同的地址。  
\- 使用 history.pushState() 会改变 referrer 的值，而在你调用方法后创建的 XMLHttpRequest 对象会在 HTTP 请求头中使用这个值。referrer的值则是创建 XMLHttpRequest 对象时所处的窗口的URL。  

**参数(三个参数)**：  

　　**1. 状态对象（state object）**： 一个JavaScript对象，与用pushState()方法创建的新历史记录条目关联。无论何时用户导航到新创建的状态，popstate事件都会被触发，并且事件对象的state属性都包含历史记录条目的状态对象的拷贝。  

任何可序列化的对象都可以被当做状态对象。因为FireFox浏览器会把状态对象保存到用户的硬盘，这样它们就能在用户重启浏览器之后被还原，我们强行限制状态对象的大小为640k。如果你向pushState()方法传递了一个超过该限额的状态对象，该方法会抛出异常。如果你需要存储很大的数据，建议使用sessionStorage或localStorage。  

　　**2. 标题（title）**：FireFox浏览器目前会忽略该参数，虽然以后可能会用上。考虑到未来可能会对该方法进行修改，传一个空字符串会比较安全。或者，你也可以传入一个简短的标题，标明将要进入的状态。  

　　**3. 地址（URL）**：新的历史记录条目的地址。浏览器不会在调用pushState()方法后加载该地址，但之后，可能会试图加载，例如用户重启浏览器。新的URL不一定是绝对路径；如果是相对路径，它将以当前URL为基准；传入的URL与当前URL应该是同源的，否则，pushState()会抛出异常。该参数是可选的；不指定的话则为文档当前URL。

注意：pushState()方法永远不会触发hashchange事件，即便新的地址只变更了hash。

#### 1.2 history.replaceState()
\- history.replaceState()操作类似于history.pushState()，不同之处在于replaceState()方法会修改当前历史记录条目而并非创建新的条目。  
\- 效果类似window.location.replace(url)  
\- 用法和参数同pushState()  

### 2. 事件
#### 2.1 popstate事件
\- 每当激活的历史记录发生变化时，都会触发popstate事件（调用pushState()和replaceState()不会触发popstate事件）。如果被激活的历史记录条目是由pushState所创建，或是被replaceState方法影响到的，popstate事件的状态属性将包含历史记录的状态对象的一个拷贝。  
\-  popstate事件的事件对象有一个 state 属性，这个属性就包含着当初以第一个参数传递给 pushState() 的状态对象。得到这个状态对象后，必须把页面重置为状态对象中的数据表示的状态（因为浏览器不会自动为你做这些）
```js
window.onpopstate=function(event){
  var state=event.state;
  if (state){ //第一个页面加载时 state 为空
    processState(state); //processState为自定义函数
  }
}
```
# 相关方法和事件
## 1. location.hash 和 hashchange
#### 1.1 location.hash
\- 读取或修改URL 的锚部分（从 # 号开始的部分）  
\- 修改hash，浏览历史会添加一条记录( history.length++ , 但不会刷新或加载页面 ) ,并且会触发hashchange和popstate事件
#### 1.2 hashchange
\- 类似onpopstate，只是在location.hash变化时才会触发  
```
window.onhashchange = fn;
或者
<body onhashchange="fn();">
```
## 2. location.replace(url)
\- 这个方法只接受一个参数，即要导航到的 URL；结果虽然会导致浏览器位置改变，但不会在历史记录中生成新记录。在调用 replace() 方法之后，用户不能回到前一个页面  

**参考**：
- 《高三》 8.5、16.4
- https://developer.mozilla.org/zh-CN/docs/DOM/Manipulating_the_browser_history
