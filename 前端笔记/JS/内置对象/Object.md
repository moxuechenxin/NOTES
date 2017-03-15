## 创建对象
#### 1. 字面量

```js
var obj = {
    name : "paul",
    age : 18 
};
```
```js
// ES6
var name = "paul";
var age = 18;
var obj = {name,age} 
```
#### 2. 调用构造函数  
##### 2.1 使用`Object`构造函数
可以不使用`new`直接调用`Object()`
```js
var obj = new Object();
obj.name = "paul";
obj.age = 18;
```
##### 2.2 使用自定义构造函数
必须使用`new`调用构造函数
```js
function Obj(){
  this.name = "paul";
  this.age = 18;
}
var obj = new Obj();
```

#### 3. 使用Object.create()

```js
var obj = Object.create(null);
obj.name = "paul";
obj.age = 18;
```

## 属性访问

```js
var obj = {
    name : "paul",
    age : 18 
};

obj.name === obj["name"];  //true
```
### `getter` 和 `setter` 

```js
var obj = {
    firstname : "ji",
    lastname : "zheng",
    get name(){
        return this.firstname + this.lastname;
    },
    set name(val){
        this.firstname = val.split(" ")[0];
        this.lastname = val.split(" ")[1];
    }
}
```

## 属性遍历
### for...in 循环
循环遍历对象可枚举的属性(键)（包括继承的）

### for...of 
(ES6)  

## 运算符
运算符 | 用法 | 描述
---|---|---
in | prop in obj | 判断对象是否含有指定（包括继承来的和不可枚举）的属性（返回true或false）
delete | delete obj.propName | 删除对象的某个属性
instanceof | obj instanceof constructor | 检测 `constructor.prototype` 是否存在于参数 `obj` 的原型链上

## Object 对象属性
### 静态属性
属性 | 描述
---|---
Object.prototype |	使您有能力向对象添加属性和方法。

### 实例属性
属性 | 描述
---|---
constructor	| （继承自Object.prototype.constructor）返回对创建此对象的数组函数的引用。
\_\_proto\_\_ | 返回构造函数的prototype属性指向的对象 `obj.__proto__ === obj.constructor.prototype`

**注意**：
- 构造函数含有一个`prototype`属性，指向一个对象（该对象上的属性和方法会被实例继承）。该对象含有一个`constructor`属性，指向构造函数本身。

## Object 对象方法
### 静态方法
方法 | 参数 |描述
---|---|---
Object.create() | (proto [, properties ]) `properties`为属性描述符对象 |(ES5) 创建一个拥有指定原型和若干个指定属性的对象（返回新创建的对象）
Object.getOwnPropertyNames() | (obj) |(ES5)  返回一个由指定对象的所有`自身属性`的属性名（包括不可枚举属性）组成的数组
Object.keys() | (obj) |(ES5)  返回一个由指定对象的所有`自身属性`中`可枚举`的属性名组成的数组
Object.getPrototypeOf() | (obj) |(ES5) 返回对象的原型（也就是内部属性`[[prototype]]`或`__proto__`指向的对象）
Object.setPrototypeOf() | (obj , proto) |(ES6) 设置对象的原型（等同于修改`__proto__`属性值），返回该对象
Object.defineProperty() | (obj , prop , descriptor) |(ES5)  在对象上添加或修改一个属性 （返回该对象）
Object.defineProperties() | (obj , props) |(ES5)  在对象上添加或修改一个或多个属性 （返回该对象）
Object.getOwnPropertyDescriptor() | (obj , prop) |(ES5)  返回指定对象上一个自有属性对应的属性描述符,如果没有该属性则返回undefined
Object.getOwnPropertyDescriptors() | (obj) |(ES7) 返回一个对象的所有自身属性的描述符,如果没有任何自身属性，则返回空对象
Object.freeze() | (obj) |(ES5)  冻结对象的所有自身属性都不可能以任何方式被修改 (返回该对象)
Object.isFrozen() | (obj) |(ES5) 判断对象是否被冻结 (返回true或false)
Object.seal() | (obj) |(ES5) 密封对象，不能新增或删除属性，不能修改属性的描述符属性（descriptors），只能修改已有属性 (返回该对象)
Object.isSealed() | (obj) |(ES5) 判断一个对象是否是密封的 (返回true或false)
Object.preventExtensions() | (obj) |(ES5)  让一个对象变的不可扩展，也就是永远不能再添加新的属性 (返回该对象)
Object.isExtensible() | (obj) |(ES5) 判断一个对象是否是可扩展的 (返回true或false)：对象调用`freeze()`、`seal()`和`preventExtensions`后该属性均为false
Object.is() | (value1 , value2) |(ES6) 用来判断两个值是否是同一个值 (返回true或false)


### 实例方法
方法 | 参数 |描述
---|---|---
toString() | 无 |  返回一个表示该对象的字符串
hasOwnProperty() | (prop) `prop`表示要检测的属性 | 判断某个对象是否含有指定（非继承）的属性 (返回true或false)
propertyIsEnumerable() | (prop) | 判断该对象指定的属性是否是`可枚举`的`自身属性` (返回true或false)
prototypeObj.isPrototypeOf(obj) |  |  测试一个对象是否存在于另一个对象的原型链上 (返回true或false)


## Object 方法详解
### 静态方法
#### Object.create()
(ES5) 创建一个拥有指定原型和若干个指定属性的对象
**语法**：

```js
Object.create(proto, [ properties ])
```
**参数**：

- **proto**  
一个对象，作为新创建对象的原型
- **properties**  
 可选。该参数对象是一组属性与值，该对象的属性名称将是新创建的对象的属性名称，值是属性描述符（这些属性描述符的结构与Object.defineProperties()的第二个参数一样）。  
**注意**：该参数对象不能是 undefined，另外只有该对象中自身拥有的可枚举的属性才有效，也就是说该对象的原型链上属性是无效的。

```js
var o;

// 创建一个原型为null的空对象
o = Object.create(null);


o = {};
// 以字面量方式创建的空对象就相当于:
o = Object.create(Object.prototype);


o = Object.create(Object.prototype, {
  // foo会成为所创建对象的数据属性
  foo: { writable:true, configurable:true, value: "hello" },
  // bar会成为所创建对象的访问器属性
  bar: {
    configurable: false,
    get: function() { return 10 },
    set: function(value) { console.log("Setting `o.bar` to", value) 
    }
}})
```

#### Object.getOwnPropertyNames()
(ES5) 返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性,不包括继承的属性）组成的数组
**语法**：

```js
Object.getOwnPropertyNames(obj)
```
返回的数组中枚举属性的顺序与通过 `for...in` 循环（或 `Object.keys`）迭代该对象属性时一致。 数组中不可枚举属性的顺序未定义。

```js
var arr = ["a", "b", "c"];
console.log(Object.getOwnPropertyNames(arr).sort()); // ["0", "1", "2", "length"]

var obj = {
    name : "paul",
    age : 18
}
console.log(Object.getOwnPropertyNames(obj)); // ["name","age"]
```

#### Object.keys()
(ES5)  返回一个由指定对象的所有`自身属性`中`可枚举`的属性名组成的数组
**语法**：

```js
Object.keys(obj)
```
返回的数组中属性名的排列顺序和使用for-in循环遍历该对象时返回的顺序一致 (顺序一致不包括数字属性)（两者的主要区别是 for-in 还会遍历出一个对象从其原型链上继承到的可枚举属性）

```js
var arr = ["a", "b", "c"];
console.log(Object.keys(arr)); // ["0","1","2"]

// 类数组对象
var obj = { 
    name : "paul",
    age : 18
};
console.log(Object.keys(obj)); // ["name","age"]
```

#### Object.getPrototypeOf()
(ES5) 返回指定对象的原型（也就是内部属性`[[prototype]]`或`__proto__`指向的对象）
**语法**：
```js
Object.getPrototypeOf(obj)
```
```js
var proto = {};
var obj = Object.create(proto);
Object.getPrototypeOf(obj) === proto; // true
Object.getPrototypeOf(obj) === obj.__proto__; // true
```

#### Object.setPrototypeOf()
(ES6) 设置对象的原型（等同于修改`__proto__`属性值）,返回该对象
**语法**：
```js
Object.setPrototypeOf(obj,proto)
```
**参数**：

- **obj**  
将被设置原型的对象
- **proto**  
该对象新的原型(可以是一个对象或者null)

```js
var obj = Object.setPrototypeOf({}, null); // {}
obj.toString  // undefined
```

#### Object.freeze()
(ES5) 冻结对象的所有自身属性都不可能以任何方式被修改 (返回该对象)
**语法**：
```js
Object.freeze(obj)
```
**注意**：
数据属性的值不可更改，访问器属性（有getter和setter）也同样（但由于是函数调用，给人的错觉是还是可以修改这个属性）。  
如果一个属性的值是个对象，则这个对象中的属性是可以修改的，除非它也是个冻结对象。

```js
var obj = {
    name : "paul",
    age : 18
}
Object.freeze(obj);

obj.gender = "male";  // 静默失败（严格模式下抛错）
delete obj.name; // false（严格模式下抛错）
obj.age = 20;   // 静默失败（严格模式下抛错）

```

#### Object.seal()
(ES5) 密封对象，不能新增或删除属性，不能修改属性的描述符属性（descriptors），只能修改已有属性 (返回该对象)
**语法**：
```js
Object.seal(obj)
```
**注意**：
密封一个对象会让这个对象变的不能添加新属性，且所有已有属性会变的不可配置。属性不可配置的效果就是属性变的不可删除，以及一个数据属性不能被重新定义成为访问器属性，或者反之。但属性的值仍然可以修改。

```js
var obj = {
    name : "paul",
    age : 18
}
Object.seal(obj);

obj.gender = "male";  // 静默失败（严格模式下抛错）
delete obj.name; // false（严格模式下抛错）
obj.age = 20;   // 修改成功
```

#### Object.preventExtensions()
(ES5) 让一个对象变的不可扩展(`isFrozen`、`isSealed`均返回false)，也就是永远不能再添加新的属性 (返回该对象)
**语法**：
```js
Object.preventExtensions(obj)
```
**注意**：
1. 不可扩展的对象的属性通常仍然可以被删除
2. 只能阻止一个对象不能再添加新的自身属性，仍然可以为该对象的原型添加属性
3. 在 ECMAScript 5 中可扩展的对象可以变得不可扩展，但反过来不行
4. 在对象上调用`Object.freeze(obj)`、`Object.seal(obj)`和`Object.preventExtensions()`其中一个方法后，该对象均变为不可扩展

```js
var obj = {
    name : "paul",
    age : 18
}
Object.preventExtensions(obj);

obj.gender = "male";  // 静默失败（严格模式下抛错）
delete obj.name; // true
obj.age = 20;   // 修改成功
```

#### Object.is()
(ES6) 用来判断两个值是否是同一个值 (返回true或false)
**语法**：
```js
Object.is(value1 , value2);
```
**注意**：
1. 该方法是`===`比较的扩展，可以区分`-0`和`+0`,另外将`NaN`和`NaN`视为相等的

```js
Object.is(1,"1"); // false
Object.is(-0,+0); // false
Object.is(NaN,NaN); // true
```

### 实例方法
#### toString()
返回一个表示该对象的字符串
```js
Object.prototype.toString.call(1);  // [object Number]
Object.prototype.toString.call("abc");  // [object String]
Object.prototype.toString.call(true);  // [object Boolean]
Object.prototype.toString.call(undefined);  // [object Undefined]
Object.prototype.toString.call(null);  // [object Null]

Object.prototype.toString.call({});  // [object Object]
Object.prototype.toString.call([]);  // [object Array]
Object.prototype.toString.call(function(){});  // [object Function]

Object.prototype.toString.call(new Date());  // [object Date]
Object.prototype.toString.call(Math);  // [object Math]
//.....
```

#### hasOwnProperty()
判断某个对象是否含有指定的属性(非继承)；注意与`in`操作符区别  

**语法**：
```js
obj.hasOwnProperty(prop)
```
**参数**：

- **prop**  
要检测的`属性字符串`名称或者`Symbol（ES6）`

```js
var obj = new Object();
obj.prop = 'exists';
obj.hasOwnProperty('prop');             // 返回 true
obj.hasOwnProperty('toString');         // 返回 false
obj.hasOwnProperty('hasOwnProperty');   // 返回 false

'prop' in obj; // 返回 true
'toString' in obj; // 返回 true
'hasOwnProperty' in obj; // 返回 true
```
```js
// 遍历一个对象的所有自身属性
var obj = {
    name: 'paul',
    age: 18
};

for (var key in obj) {
    if (obj.hasOwnProperty(key)) {
        console.log(key,obj[key]);
    }
}
```

#### propertyIsEnumerable()
判断该对象指定的属性是否是`可枚举`的`自身属性` 
**语法**：

```js
obj.propertyIsEnumerable(prop)
```
**参数**：

- **prop**  
要检测的`属性字符串`名称或者`Symbol（ES6）`

```js
var a = ['is enumerable'];

a.propertyIsEnumerable(0);          // 返回 true
a.propertyIsEnumerable('length');   // 返回 false

Math.propertyIsEnumerable('random');   // 返回 false
this.propertyIsEnumerable('Math');     // 返回 false
```

#### isPrototypeOf()
用于测试一个对象是否存在于另一个对象的原型链上
**语法**：

```js
prototypeObj.isPrototypeOf(obj)
```
**参数**：

- **prototypeObj**  
  在原型链上游的对象
- **obj**
  以`obj`为起点沿原型链往上搜寻是否能找到`prototypeObj`

**注意**：与`instanceof`的使用区别

```js
function Fn1() {
  // . . .
}

function Fn2() {
  // . . .
}
Fn2.prototype = new Fn1();

var fn2 = new Fn2();

Fn1.prototype.isPrototypeOf(fn2); // true
fn2 instanceof Fn1; // true
```



**参考**：
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object