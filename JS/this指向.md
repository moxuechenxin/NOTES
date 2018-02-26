## 一般函数中的this
### 1. 作为普通函数调用
this指向全局，浏览器环境中即window对象 (严格模式下指向undefined)
```js
var name = "paul";
function sayHi () {
    console.log("Hi,I'm " + this.name);
}

sayHi();  // "Hi,I'm paul"

function func(){ 
    "use strict"
    console ( this ); // 输出undefined
}

func();
```

### 2. 作为对象的方法调用
```
var obj = {
    name: "paul",
    sayHi: function(){
        console.log("Hi,I'm " + this.name);
    }
}

obj.sayHi(); // "Hi,I'm paul"

var sayHiFn = obj.sayHi;
sayHiFn();  // this指向window，因为sayHiFn的调用者是window
```

## 构造函数中的this (new操作符)
**new 操作符**
```js
// 构造函数
function Fn(name){
    this.name = name;
}
```
```js
var fn = new Fn("paul");

// 相当于以下过程
function newConstructor () {
    var obj = new Object();
    var Constructor = [].shift.call(arguments); // 取得（并在arguments中删除）第一个参数，即构造函数
    obj.__proto__ = Constructor.prototype;
    var ret = Constructor.apply(obj, arguments); // 此时的arguments已删除第一个参数（constructor）
    return typeof ret === 'object' ? ret : obj;
}

var fn = newConstructor(Fn, "paul");
```

### 1. 构造函数没有返回值
构造函数没有返回值时，使用new运算符后返回this，this指向new出来的实例对象
```js
function Fn(){
    this.a=22;
}
var obj=new Fn();
console.log(obj); // Fn{ a:22 } obj是一个对象
console.log(obj.a); //22
```
### 2. 构造函数有返回值，返回值不是对象
构造函数返回值不是对象时，使用new运算符后返回this，this指向指向new出来的实例对象
```js
function Fn(){
    this.a=33;
    return this.b;
}
var obj=new Fn();
console.log(obj); // Fn{ a:33 } obj是一个对象
console.log(obj.a); //33
```
### 3. 构造函数有返回值，返回值是对象
构造函数返回值是对象时，使用new运算符后返回该对象
```js
function Fn(){
    this.a=44;
    return {a:55};
}
var obj=new Fn();
console.log(obj); // Object{ a:55 }
console.log(obj.a); // 55
```
### 4.使用bind的情况
函数调用bind方法，绑定this为作为bind方法参数的对象，但使用new操作符（将函数作为构造函数），this绑定失效
```js
function Fn(){
    this.a=66;
    return this.b;
}
var fn=Fn.bind( { b:77 } ); // 将Fn的this绑定为对象{ b:77 } ，bind是ES5提供的方法，IE9以下不支持
console.log( fn );// function () { [native code] }
console.log( fn() ); // 77
var obj=new fn(); // new运算符后的结果不受bind方法的影响,this.b为undefined不是对象，属于情况2（null相同）
console.log( obj ); // Fn { a:66 }
```


