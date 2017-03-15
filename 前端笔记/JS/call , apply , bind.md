## 参数
####  fn.call ( thisObj,agr1,agr2...)
####  fn.apply ( thisObj, array )
\- IE9+  
\- 第二个参数可以为类数组（ES5新增：IE10+）
####  fn.bind ( thisObj,agr1,agr2...)
\- IE9+  
\- 返回this值绑定为thisObj的新函数，无论该新函数怎么被调用，this始终绑定为thisObj

### bind's Polyfill

```js
if (!Function.prototype.bind) {
  Function.prototype.bind = function (oThis) {
    if (typeof this !== "function") {
      // closest thing possible to the ECMAScript 5
      // internal IsCallable function
      throw new TypeError("Function.prototype.bind - what is trying to be bound is not callable");
    }

    var aArgs = Array.prototype.slice.call(arguments, 1);
    var fToBind = this;
    var fNOP = function () {};
    var fBound = function () {
          return fToBind.apply( this instanceof fNOP ? this : oThis || this, aArgs.concat(Array.prototype.slice.call(arguments)) );
    };

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();

    return fBound;
  };
}

// 简易版
Function.prototype.bind = function( context ){
    var self = this; // 保存原函数
    return function(){ // 返回一个新的函数
        return self.apply( context, arguments ); // 执行新的函数的时候，会把之前传入的 context当作新函数体内的 this
    }
};
```

### 相同点
如果第一个参数为null或者undefined的话，在非严格模式下，实际上绑定到的是全局对象，这一点对三者都适用。在严格模式下，this值不做转换，即为null或undefined

### 一些用法
- 调用原生对象方法

```js
/*
    使用类似 [].slice.call(arrLike) 或 [].slice.apply(arrLike)
    用法同数组调用相应方法
*/

// 遍历类数组
[].forEach.call(arrLike, function(item, index){
    console.log(item)
})

/*
    使用类似 ({}).toString.call(anotherObj) 或 ({}).toString.call(anotherObj)
    用法同Object调用相应方法
*/

/*
    ....
*/


```

- 将类数组转换为数组   

```js
// 方法一
Array.prototype.slice.call(arguments);

// 方法二
var unboundSlice = Array.prototype.slice;  
var slice = Function.prototype.call.bind(unboundSlice);  
slice(arguments);
```

- 获取数组中最大的数值  

```js
// ES3之前 Math.max() 只能传递两个参数
Math.max.apply(null, [1, 2, 3, 5, 7]); 
```

- 利用bind实现柯里化      
      
```js
function fn1(a,b){
    console.log(a,b);
}
var fn2=fn1.bind(window,1);
fn2(); //1 undefined
```

**参考**：    
- **this**:  https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this  
- **bind()**: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bin;;