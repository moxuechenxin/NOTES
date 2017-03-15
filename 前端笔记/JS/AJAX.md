# 简易AJAX
### 1. 创建XHR对象
```js
var xhr;
if(window.XMLHttpRequest){
    xhr=new XMLHttpRequest(); //IE7+
}else{
    xhr=new ActiveXObject("Microsoft.XMLHTTP");//IE6
}
```
### 2. 发送请求
```js
// method：发送请求方法GET、POST
// async：请求同步false/异步true（默认true）
xhr.open(method,url,async)
// 设置返回数据类型
xhr.responseType = "";
//（XHR2）支持跨域发送cookies（服务端需设置：header("Access-Control-Allow-Credentials: true")）
xhr.withCredentials = true; 
// 设置请求头信息（必须在open方法之后）
xhr.setRequestHeader(key,value)
// GET请求时可无参数或为null；POST一般要填参数
xhr.send(string)
```

### 3. 获取服务器响应
```js
// xhr.responseText :获取字符串形式的响应数据
// xhr.responseXML :获取XML形式的响应数据
// xhr.status 和 xhr.statusText：以数字和文本形式返回HTTP状态码
// xhr.getAllResponseHeader() :获取所有的响应报头
// xhr.getResponseHeader() :查询响应中的某个字段的值

```
### 4.监听请求状态
```js
// xhr.readyState属性:
// 0 :请求未初始化,open还没有调用
// 1 :服务器连接已建立，open已经调用了
// 2 :请求已接收，也就是接收到头信息了
// 3 :请求处理中，也就是接收到响应主体了
// 4 :请求已完成，且响应已就绪，也就是响应完成了

// xhr.onreadystatechange事件
```
### 5.abort()方法
XMLHttpRequest.abort() 方法将终止请求，如果该请求已被发出。  
当一个请求被终止，它的 readyState 属性将被置为0（ UNSENT ），但是并不触发 readystatechange 事件

参考：https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/abort

### 完整实例
```js
var xhr;
try{
   xhr= new XMLHttpRequest();
}catch(e){
   xhr= new ActiveXObject('Microsoft.XMLHTTP'); //IE6
}

//绑定请求状态变化事件
xhr.onreadystatechange=function(){
/*
    // 获取服务器时间 (通过返回头)
    if (xhr.readyState === 3) {
      var now = xhr.getResponseHeader('Date');
      now=(new Date(now).valueOf());
      //获取到服务器时间后终止请求
      xhr.abort();
    }
    
*/
    if(xhr.readystate==4){
        if(xhr.status==200){
            alert(xhr.responseText);
        }else{

        }
    }
}

//get方式发送数据
xhr.open('GET','1_get.php?name='+encodeURI("郑骥")+'&age=25&'+new Date().getTime(),true);
xhr.send();
/*
* get方式的两个问题：
* 1.缓存问题，使用时间戳解决
* 2.中文编码问题，使用encodeURI进行编码
*/

//post方式发送数据
xhr.open('POST','2_post.php',true);
xhr.setRequestHeader('content-type','application/x-www-form-urlencoded'); //申明发送数据类型
xhr.send('name=郑骥&age=25')
/*
*post方式注意：
* 1.必须设置请求头信息
* 2.发送的数据作为send()的参数
*/

//完整实例 END
```
**创建AJAX过程**：  
1. 创建XMLHttpRequest对象,也就是创建一个异步调用对象      
2. 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息      
3. 设置响应HTTP请求状态变化的函数  
4. 发送HTTP请求  
5. 获取异步调用返回的数据     
6. 使用JavaScript和DOM实现局部刷新   

### 封装对象
```js
/**
  *  opts : {
    method: '',
    url: '',
    header: '',
    data: '',
    success:function(responseText){},
    fail:function(){}
  }
*/
function ajax(opts){
    if(!(this instanceof ajax)){
      return new ajax(opts);
    }
    var xhr = new XMLHttpRequest();
    //绑定请求状态变化事件（可使用'load'和'error'事件替代）
    xhr.onreadystatechange = function(){
      if(xhr.readyState === 4){
        if(xhr.status === 200){
          opts.success && opts.success(xhr.responseText);
        }else{
          opts.fail && opts.fail(xhr.responseText);
        }
      }
    }
    // 设置返回数据类型
    if(opts.responseType){
        xhr.responseType = opts.responseType;
    }
    
    if(opts.method && opts.method.toLowerCase() === "post"){
      xhr.open("POST",opts.url,true);
      xhr.setRequestHeader('content-type', opts.header || 'application/x-www-form-urlencoded');
      xhr.send( this.serialize(opts.data) );
    }else{
      xhr.open("GET",opts.url + "?" + this.serialize(opts.data) + new Date().getTime() ,true);
      if(opts.header){
          for(var key in opts.header){
              xhr.setRequestHeader(key,opts.header[key]);
          }
      }
      xhr.send(null);
    }
    return xhr;
}
ajax.prototype = {
    constructor:ajax,
    serialize:function(obj){
        var result = "";
        for(var key in obj){
            result += key + "=" + encodeURI( obj[key] ) + "&";
        }
        return result.slice(0,-1);
    }
}
```

**参考**：
- http://www.imooc.com/video/5913   
- http://www.runoob.com/ajax/ajax-tutorial.html   
- https://developer.mozilla.org/zh-CN/docs/AJAX
- https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest  

# XHR2
\-  在标准浏览器下，XMLHttpRequest对象已经是升级版本，支持了更多的特性，可以跨域了  
\-  后端需设置以下头部信息：  
`header('Access-Control-Allow-Origin: * ');`  
`header('Access-Control-Allow-Methods: POST,GET '); `  
\-  XHR2 增加很多功能，也不推荐使用onreadystatechange这个事件来监听，推荐使用onload（请求完成并成功）  
\-  兼容性：IE10+ , 低版本IE 如果想实现跨域请求，则需要使用 XDomainRequest 对象去实现

```js
var xhr;
if( typeof XDomainRequest ==="object"){
    xhr = new XDomainRequest();
    xhr.open('get', 'http://www.b.com/ajax_b1.php');
}else{
    xhr = new XMLHttpRequest();
    xhr.open('get', 'http://www.b.com/ajax_b1.php', true);
}
xhr.onload=function(){
  alert(xhr.responseText);
}
xhr.send();
```

# 进度监测


**参考**：
- https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS
- https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest
