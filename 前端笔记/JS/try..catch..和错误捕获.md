## try..catch..
try...catch语句将能引发错误的代码放在try块中，并且对应一个响应，然后有异常被抛出

**语法**

```js
try{
    // statements...
}
catch(error){
    // ....
    return catch_result_1;
}
// will always execute
finally{
    // ...
}
```

**注意**：
- catch语句和finally语句可只出现一个
- catch错误后不影响后续代码执行

```js

```


## onerror事件
#### window.onerror事件
当JavaScript运行时错误（包括语法错误）发生时，window会触发一个ErrorEvent接口的error事件，并执行window.onerror()

**语法**

```js
window.onerror = function(message,source,row,col,error){...}
```
**函数参数**
- **message**：错误信息（字符串）。
- **source**：发生错误的脚本URL（字符串）
- **lineno**：发生错误的行号（数字）
- **colno**：发生错误的列号（数字）
- **error**：Error对象（对象）

**注意**：
若该函数返回true，则阻止执行默认事件处理函数，比如控制台不输出错误信息


#### element.onerror事件
**语法**

```js
element.onerror = function(event){...}
```
`element.onerror`使用单一Event参数的函数作为其处理函数


```js
// 图片加载失败事件处理
var img = new Image();
img.onerror=function(event){
    // ....handler
}
img.src = ...;
```

**注意**：
- window.onerror并不能捕获js执行错误，也就是说js执行错误后触发error事件处理函数，后续代码依旧不能执行了

**参考**：
- [JavaScript Errors 指南](https://github.com/Jocs/jocs.github.io/issues/1)
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/try...catch
- https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalEventHandlers/onerror
- https://gist.github.com/neekey/4371159
- http://www.cnblogs.com/hustskyking/p/fe-monitor.html
