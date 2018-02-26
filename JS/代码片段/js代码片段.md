### 获取元素样式

```js
// 注意如果样式是尺寸类，且display属性为none，不能获取计算后的值，而是设置的值如百分比等
function getStyle(obj,attr){
    if(obj.currentStyle){
        return obj.currentStyle[attr];
    }else{
        return window.getComputedStyle(obj,null)[attr];
    }
}
```

### 实现类似getElementsByClassName 的功能

```js
function getByClass(el,cls){
  var start='(^|[ \n\r\t\f])'; // 匹配空白字符（空格、换行、回车、制表符、换页符）
  var end='([ \n\r\t\f]|$)';
  var reg=new RegExp(start + cls + end);
  var elList=[];
  //获取el下的所有元素
  var elements=el.getElementsByTagName("*");
  //遍历元素找出类名符合的元素
  for(var i=0,len=elements.length;i<len;i++){
    if( reg.test(elements[i].className) ){
      elList.push(elements[i]);
    }
  }
  return elList;
}
```

### 获取元素相对祖先元素或文档左上角的距离

```js
//固定定位元素offsetParent属性值为null，但offsetLeft和offsetTop属性值正常
function getPos(ele,ancestor){
  var left=0;
  var top=0;
  while( (ele.offsetParent || getStyle(ele,"position")==="fixed" ) && ele!==ancestor){
    left+=ele.offsetLeft;
    top+=ele.offsetTop;
    if( getStyle(ele,"position")==="fixed" ){
      break;
    }
    ele=ele.offsetParent;
  }
  return{
    left:left,
    top:top
  }
}
```

### 将时间转化为14位数值字符串

```js
function timeToDigit(dateStr){
  dateStr=dateStr.replace(/[^\d]+/g,"");
  var len=dateStr.length;
  if(len<14){
    dateStr=dateStr+( new Array(14-len+1).join("0"));
  }
  return dateStr;
}
```

### 将14位数值字符串转为对象

```js
/***将14位数值字符串转为对象: timeObj--时间对象 ,timeStr--指定格式时间字符串
 *
 * @params opts(对象): type--输出时间格式  time--需要转换的纯数值字符串（14位）
 *
 */
function timeFormat(opts){
  var type=opts.type;
  var time=opts.time+"";
  //type必须为格式字符串，形似 "yyyy-mm-dd hh:ff:ss"
  type=type.trim();
  //提取分隔符 separators为分隔符数组
  var separators=[];
  separators.push( type.match( /[^ymdhfs]*(?=yyyy)/ ) ? type.match( /[^ymdhfs]*(?=yyyy)/ ).join("") : "" );
  separators.push( type.match( /[^ymdhfs]*(?=mm)/ ) ? type.match( /[^ymdhfs]*(?=mm)/ ).join("") : "" );
  separators.push( type.match( /[^ymdhfs]*(?=dd)/ ) ? type.match( /[^ymdhfs]*(?=dd)/ ).join("") : "" );
  separators.push( type.match( /[^ymdhfs]*(?=hh)/ ) ? type.match( /[^ymdhfs]*(?=hh)/ ).join("") : "" );
  separators.push( type.match( /[^ymdhfs]*(?=ff)/ ) ? type.match( /[^ymdhfs]*(?=ff)/ ).join("") : "" );
  separators.push( type.match( /[^ymdhfs]*(?=ss)/ ) ? type.match( /[^ymdhfs]*(?=ss)/ ).join("") : "" );
  separators.push( type.split("ss")[1] || "" );
  //需要返回的时间单位 units为时间单位字符串
  var units=type.split(/[^a-zA-Z]+/).join("");
  //提取各单位时间
  var year=units.indexOf("y") !== -1 ? time.substr(0,4) : "";
  var month=units.indexOf("m") !== -1 ? time.substr(4,2) : "";
  var day=units.indexOf("d") !== -1 ? time.substr(6,2) : "";
  var hour=units.indexOf("h") !== -1 ? time.substr(8,2) : "";
  var min=units.indexOf("m") !== -1 ? time.substr(10,2) : "";
  var sec=units.indexOf("s") !== -1 ? time.substr(12,2) : "";
  var timeStr=separators[0]+year+separators[1]+month+separators[2]+day+separators[3]+hour+separators[4]+min+separators[5]+sec+separators[6];
  return {
    timeObj:{
      year:year,
      month:month,
      day:day,
      hour:hour,
      min:min,
      sec:sec
    },
    timeStr:timeStr
  }

}
```
### 对象中的所有属性值均转为字符串类型

```js
// obj = $obj[$key]; 用于赋值考虑
// $obj和$key只做内部使用
function toStringForObj(obj,$obj,$key){
  if(zj.typeof(obj)==="object"){
    for(var key in obj){
      toStringForObj(obj[key],obj,key);
    }
  }else if(zj.typeof(obj)==="array"){
    obj.forEach(function(item,index,array){
      toStringForObj(item,obj,index);
    })
  }else{
    if($obj){
      $obj[$key] = $obj[$key]===undefined ? "" : $obj[$key]+"";
    }
  }
}
```

### 数据类型判断

```js
function zj_typeof(data){
   var type=typeof data;
   type=type.toLowerCase();
   if( type === "object"){
     type=Object.prototype.toString.call( data ).slice(8,-1).toLowerCase();
   }
   return type;
}
```

### 类似jquery的jQuery.extend()的方法
```js
/*** obj扩展方法(类似jQuery.extend)，返回被扩展的对象扩展之后的状态,考虑数组和对象这两种情况
 *
 * @param deep (boolean) 是否深度扩展 (可省略，默认false)
 * @param origin (object) 被扩展的对象
 * @param extra (object) 用来扩展的对象
 *
 */
function extend(){
  var agrs=arguments;
  debugger;
  var deep,origin,extra;
  var length=agrs.length;
  var start=0;
  if(typeof agrs[0] === "boolean"){
    deep=agrs[0];
    origin=agrs[1];
    start=2;
    length--;
  }else{
    deep=false;
    origin=agrs[0];
    start=1;
  }
  extra=agrs[start];
  function loop(_origin,_extra){
    //分两种情况，数组或对象
    if( typeof _origin === "object" && typeof _extra === "object" ){
      if( zj_typeof(_origin) === "object" && zj_typeof(_extra) === "object" ){
        for(var key in _extra){
          //被扩展的对象/数组对应的键值没有时或origin和extra类型不同时，origin对应key设为空对象或数组
          if( ( !_origin[key] && _origin[key]!==0 )  || zj_typeof(_origin[key]) !== zj_typeof(_extra[key]) ){
            if( zj_typeof(_extra[key]) === "array" ){
              _origin[key]=[];
            }else if( zj_typeof(_extra[key]) === "object" ){
              _origin[key]={};
            }
          }

          if( deep && typeof _origin[key] ==="object" && typeof _extra[key] ==="object" ){
            loop( _origin[key],_extra[key] );
          }else{
            _origin[key]=_extra[key];
          }
        }
      }else if( zj_typeof(_origin) === "array" && zj_typeof(_extra) === "array" ){
        var originLen=_origin.length;
        var extraLen=_extra.length;
        if(originLen>extraLen){
          _origin.splice(extraLen,originLen-extraLen);
        }
        for(var i=0,len=_extra.length;i<len;i++){
          //被扩展的对象/数组对应的键值没有时或origin和extra类型不同时，origin对应key设为空对象或数组
          if( ( !_origin[i] && _origin[i]!==0 )  || zj_typeof(_origin[i]) !== zj_typeof(_extra[i]) ){
            if( zj_typeof(_extra[i]) === "array" ){
              _origin[i]=[];
            }else if( zj_typeof(_extra[i]) === "object" ){
              _origin[i]={};
            }
          }

          if( deep && typeof _origin[i] ==="object" && typeof _extra[i] ==="object" ){
            loop( _origin[i],_extra[i] );
          }else{
            _origin[i]=_extra[i];
          }
        }
      }
    }
    //检测参数个数
     start++;
     if(start>length){
       return origin;
     }
     loop(origin,agrs[start]);
  }
  loop(origin,extra);
  return origin;
}
```
 
### 对象深层复制

```js
// 1.使用上面封装的 extend(true,{},obj)

// 2.使用JSON.parse()和JSON.stringify()
function deepCopy(obj){
    return JSON.parse( JSON.stringify(obj) ); 
}
```

### 判断某节点是否是另一节点的后代

```js
/*
*Node.contains(otherNode)返回一个布尔值来表示是否传入的节点是，该节点的子节点。
*如果 otherNode 是 Node 的后代节点或是 Node 节点本身.则返回true , 否则返回 false.
*参考：https://developer.mozilla.org/zh-CN/docs/Web/API/Node/contains
*/

/*
*compareDocumentPosition() IE8+
*方法比较两个节点，并返回描述它们在文档中位置的整数。
*参考：http://www.w3school.com.cn/jsref/met_node_comparedocumentposition.asp
*/

function elContains(descendant,ancestor){
    return ancestor.contains ? ancestor.contains(descendant) :  !!(a.compareDocumentPosition(arg) & 16);
}
```

### 图片预加载
参考：http://www.jb51.net/article/32761.htm
```js
function loadImage(url, callback) { 
    var img = new Image(); //创建一个Image对象，实现图片的预下载 
    img.src = url; 
    if (img.complete) { // 如果图片已经存在于浏览器缓存，直接调用回调函数 
        callback.call(img); 
        return; // 直接返回，不用再处理onload事件 
    } 
    img.onload = function () { //图片下载完毕时异步调用callback函数。 
        callback.call(img); //将回调函数的this替换为Image对象 
    }; 
}; 

```


