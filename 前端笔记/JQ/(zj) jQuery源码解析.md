#### 面向对象
    
```
//<line 61>
jQuery = function( selector, context ) {
		// The jQuery object is actually just the init constructor 'enhanced'
		return new jQuery.fn.init( selector, context, rootjQuery );
}, //....

//<line 96>
jQuery.fn = jQuery.prototype = {
        //....
	constructor: jQuery,
	init: function( selector, context, rootjQuery ) {
    //....
    
// <line 283> Give the init function the jQuery prototype for later instantiation
jQuery.fn.init.prototype = jQuery.fn;


```

\- 调用jQuery即返回jQuery原型上的init方法的实例  
\- jQuery.fn.init的prototype对象设置为指向jQuery的prototype对象  
\- jQuery.prototype被赋值给一个对象(字面量)，该对象的constructor属性值设置为jQuery,保证了jQuery.prototype的constructor指向依然为jQuery

#### ready事件
    
```
//<line 389>
readyList.resolveWith( document, [ jQuery ] );

$(function(agr){
    console.log(this);  // document
    console.log(agr);   // jQuery
});

$(document).ready(function(){
});

$(document).on("ready",function(){
})
```
##### $.holdReady(boolean);
    \- 暂停（true）或恢复（false）**.ready()** 事件的执行
```
$.holdReady(true);
$.getScript("myplugin.js", function() {
  $.holdReady(false);
});
```


#### 函数方法
##### $.parseHTML(htmlStr [,context] [,isKeepScripts] )
\- 将字符串解析到一个DOM节点的数组中
merge(arr1,arr2)
\- 将第二个数组合并到第一个数组。
\- 数组可为类数组对象，返回格式以第一个参数为准(此种用法一般内部使用)

##### $.makeArray(obj [,results] )
\- 转换一个类似数组的对象成为真正的js数组，并返回
\- 可将基本类型转为长度为1的数组，不传参数、null和undefined转为空数组
\- 第二个参数为类数组，则obj合并到results并返回类数组格式(此种用法一般内部使用)

##### $.extend()和$.fn.extend()
\- $.extend()只传一个（对象）参数时，是为jQuery构造函数增加方法
\- $.fn.extend()是为jQuery实例（即jQuery对象）增加方法

##### var newJq=$.noConflict ( boolean );
\- 放弃用$表示jQuery函数
\- 如果传入参数为true，则也放弃用jQuery表示jQuery函数

##### $.globalEval( code )
\- 在全局上下文下执行一些JavaScript代码
each( array_or_object, function(index,el){ } )
\- 一个通用的迭代函数，它可以用来无缝迭代对象和数组。数组和类似数组的对象通过一个长度属性（如一个函数的参数对象）来迭代数字索引，从0到length - 1。其他对象通过其属性名进行迭代
\- 回调函数中：返回false来终止迭代，返回非false相当于一个循环中的continue语句
\- 注意区别于$(selector).each()，后者用于遍历一个jQuery对象

##### $.inArray(item,arr [,fromIndex] )
\- ES5实现了数组的indexof方法，用法同字符串的indexof方法
\- 源码:[ ].indexOf.call(arr,item [,fromIndex])

##### var newArr = $.grep( array, function(item, index){} [, invert ] )
\- 返回包含回调函数返回值为true的数组项的数组
\- 若invert值为true，则取数组中返回值为true的项
\- 类似ES5的 [ ].filter(function(item,index,array) { })

##### var newArr = $.map( array, function(item, index){ } )
\- 将一个数组中的所有元素转换到另一个数组中
\- 类似ES5的[ ].map(function(item,index,array){ })