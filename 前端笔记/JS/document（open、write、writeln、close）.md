## 方法

### open()
**定义**：  
打开一个流，以收集来自任何 document.write() 或 document.writeln() 方法的输出；  
该方法将擦除当前 HTML 文档的内容，开始一个新的文档，新文档用 write() 方法或 writeln() 方法编写。

**语法**  
`document.open(mimetype,replace)`

**参数**
- mimetype  
 可选。规定正在写的文档的类型。默认值是 "text/html"。

- replace  
 可选。当此参数设置后，可引起新文档从父文档继承历史条目。

**注意**  
调用 `open()` 方法打开一个新文档并且用 `write()` 方法设置文档内容后，必须记住用 `close()` 方法关闭文档，并迫使其内容显示出来

### window.open()
**定义**：  
window.open() 方法用于打开一个新的浏览器窗口或查找一个已命名的窗口。

**语法**  
`window.open(URL,name,features,replace)`

**参数**
- URL  
 可选的字符串。声明了要在新窗口中显示的文档的 URL。如果省略了这个参数，或者它的值是空字符串，那么新窗口就不会显示任何文档。

- name  
 可选的字符串。该字符串是一个由逗号分隔的特征列表，其中包括数字、字母和下划线，该字符声明了新窗口的名称。这个名称可以用作标记 <a> 和 <form> 的属性 target 的值。如果该参数指定了一个已经存在的窗口，那么 open() 方法就不再创建一个新窗口，而只是返回对指定窗口的引用。在这种情况下，features 将被忽略。

- features  
可选的字符串。声明了新窗口要显示的标准浏览器的特征。如果省略该参数，新窗口将具有所有标准特征。

- replace  
 可选的布尔值。规定了装载到窗口的 URL 是在窗口的浏览历史中创建一个新条目，还是替换浏览历史中的当前条目

**打开新窗口被拦截**  
解决办法：
- 同域路径窗口不会被拦截
- 先打开空窗口，然后修改新窗口url地址
```js
var newWin = window.open();
// ajax获取url地址数据后,修改新窗口页面路径
newWin.location.href = "http://www.baidu.com";

```

**注意**  
注意window.open() 和 document.open()的区别

### write() 和 writeln()
**定义**：  
write() 方法可向文档写入 HTML 表达式或 JavaScript 代码。
可列出多个参数(exp1,exp2,exp3,...) ，它们将按顺序被追加到文档中。  
writeln() 方法与 write() 方法作用相同，外加可在每个表达式后写一个换行符。  

**语法**  
`document.write(exp1,exp2,exp3,....)`

**注意**  
我们通常按照两种的方式使用 write()方法：
- 在使用该方在文档中输出 HTML
- 在调用该方法的的窗口之外的窗口、框架中产生新文档。  
在第二种情况中，请务必使用 close() 方法来关闭文档。

### close()
**定义**：  
close() 方法可关闭一个由 document.open 方法打开的输出流，并显示选定的数据。

**语法**  
`document.close()`

**注意**  
1. 该方法将关闭 open() 方法打开的文档流，并强制地显示出所有缓存的输出内容。  
2. 如果您使用 write() 方法动态地输出一个文档，必须记住当你这么做的时候要调用 close() 方法，以确保所有文档内容都能显示。
3. 一旦调用了 close()，就不应该再次调用 write()，因为这会隐式地调用 open() 来擦除当前文档并开始一个新的文档。

### 应用

#### 在新窗口打开新的文档，并添加一些文本

```js
var newWin=window.open();

var txt1 = "This is a new window.";
var txt2 = "This is a test.";
newWin.document.open("text/html","replace");
newWin.document.writeln(txt1);
newWin.document.write(txt2);
newWin.document.close();
```

**参考**：
- http://www.w3school.com.cn/jsref/dom_obj_document.asp