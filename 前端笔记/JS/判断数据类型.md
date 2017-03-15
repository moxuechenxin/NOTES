## 1. 相关方法
#### 1.1 typeof 
\- 检查基本类型(除null)和函数  

```js
typeof 100  // "number"
typeof NaN  // "number"
typeof "name" // "string"
typeof true // "boolean"
typeof undefined // "undefined"
typeof function(){} // "function" 

typeof null // "object"
typeof new Object() // "object"
typeof [1,2] // "object"

```
#### 1.2 instanceof
\- 判断对象类型  
\- *object* instanceof *constructor*  
\- 检测 constructor.prototype 是否存在于参数 object 的原型链上  
\- 不同window或iframe间的对象类型检查**不能**使用instanceof ！

```js
[1,2] instanceof Array === true //true
[1,2] instanceof Object === true //true
var obj={};
obj instanceof Object === true //true
```
#### 1.3 constructor
\- 判断对象类型  
\- 不可靠，因为对象的constructor属性可修改  
\- 不同window或iframe间的对象类型检查**不能**使用constructor 
```js
[].constructor === Array // true
var obj={};
obj.constructor === Object // true
```

#### 1.4 Object.prototype.toString.call( obj );
\- 任意类型都可判断  
\- Object.prototype.toString.call( obj ).slice(8,-1).toLowerCase();

```js
Object.prototype.toString.call( [1,2] ) // "[object Array]"
Object.prototype.toString.call( {} ) // "[object Object]"
Object.prototype.toString.call( null ) // "[object Null]"; IE6/7/8 返回"[object Object]"
Object.prototype.toString.call( undefined ) // "[object Undefined]"; IE6/7/8 返回"[object Object]"
```
## 综合判断方法

```js
function getType(agr){
    var type=typeof agr;
    if(type==="object"){
        type=Object.prototype.toString.call( obj ).slice(8,-1).toLowerCase();
    }
    //兼容IE6/7/8
    if(agr===null){
        type=null;
    }else if(agr===undefined){
        type=undefined;
    }
    return type;
}
```


   