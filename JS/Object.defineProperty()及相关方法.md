## Object.defineProperty()  <ES5> IE9+
在一个对象上定义一个新属性，或者修改一个已经存在的属性， 并**返回这个对象**

#### 语法

```js
Object.defineProperty(obj, prop, descriptor)
```
#### 参数
- **obj**  
需要定义属性的对象。
- **prop**  
需定义或修改的属性的名字(字符串)。
- **descriptor**  
将被定义或修改的属性的描述符（对象类型）。  
**相关方法**：`Object.getOwnPropertyDescriptor(obj,prop)`

#### 描述符分类（只能选其一）
- **数据描述符**

    可选键值：  
    
    **value**  
    该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。  
    
    **writable**   
    当且仅当该属性的 writable 为 true 时，该属性才能被赋值运算符改变。默认为 false。  
    
    **enumerable**   
    当且仅当该属性的 enumerable 为 true 时，该属性才能够出现在对象的枚举属性中。默认为 false。**为false时，该属性不能在 `for...in` 循环和 `Object.keys()` 中被枚举**
    
    **configurable**  
    当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，也能够被删除。默认为 false。**为false时，除了writable（只能从true改为false），其他特性（描述符）都不能修改，并且数据和存取描述符也不能相互切换**

  
- **存取描述符（访问器描述符）**

    可选键值：
    
    **get**  
    个给属性提供 getter 的方法，如果没有 getter 则为 undefined。该方法返回值被用作属性值。默认为 undefined。  
    
    **set**   
    一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。该方法将接受唯一参数，并将该参数的新值分配给该属性。默认为 undefined。 
    
    **enumerable**   
    同数据描述符
    
    **configurable**  
    同数据描述符
    
**注意**：
- **通过赋值添加对象属性时，writable、enumerable、configurable 默认值为 true**
- **但用Object.defineProperty()添加属性时默认值为false**
- **使用Object.defineProperty()修改属性时默认值同之前**


1. **数据描述符**

```js
var obj = {};

/* 通过Object.defineProperty()添加属性 */
Object.defineProperty(obj,"name",{
    value : "paul",  // 未指定writable，默认为false
    enumerable : true, 
    configurable : true
});

Object.getOwnPropertyDescriptor(obj,"name"); // {value: "paul", writable: false, enumerable: true, configurable: true}


console.log(obj.name); // "paul"

/*修改属性*/

// 1. 赋值修改
// 由于writable特性为false，不能通过赋值修改属性值
obj.name = "paul2";  
console.log(obj.name); // "paul"

// 2. 通过Object.defineProperty()修改
// 即使writable为false，但configurable为true则可以通过Object.defineProperty()修改
Object.defineProperty(obj,"name",{
    value : "paul3"
});
console.log(obj.name); // "paul3"

Object.getOwnPropertyDescriptor(obj,"name"); // {value: "paul3", writable: false, enumerable: true, configurable: true}

/* 通过赋值添加属性 */
obj.age = 18;
Object.getOwnPropertyDescriptor(obj,"age"); // {value: 18, writable: true, enumerable: true, configurable: true}

// 通过Object.defineProperty()修改属性
Object.defineProperty(obj,"age",{
    value : 28
}); 
console.log(obj.age); // 28

Object.getOwnPropertyDescriptor(obj,"age"); // {value: 28, writable: true, enumerable: true, configurable: true}

```
2. **存取描述符（访问器描述符）**

```js
function Archiver() {
  var temperature = null;
  var archive = [];

  Object.defineProperty(this, 'temperature', {
    get: function() {
      console.log('get!');
      return temperature;
    },
    set: function(value) {
      temperature = value;
      archive.push({ val: temperature });
    }
  });

  this.getArchive = function() { return archive; };
}

var arc = new Archiver();
arc.temperature; // 'get!'
arc.temperature = 11;
arc.temperature = 13;
arc.getArchive(); // [{ val: 11 }, { val: 13 }]


```

**关于存取（访问器）属性（ES5）**

```js
// 1. 直接定义
var circle={
    r:10,  //注意句末逗号
    get size(){
        return this.r*this.r*Math.PI;
    },   //注意句末逗号
    set size(val){
        this.r=Math.sqrt(val/(Math.PI));
    }
}

Object.getOwnPropertyDescriptor(circle,"size"); // {enumerable: true, configurable: true,set: function()..,get: function()...}
 
console.log(circle.r); // 10
console.log(circle.size); // 314.159....
circle.size=31400;
console.log(circle.r); // 99.97....

// 2. 通过Object.defineProperty()定义


Object.defineProperty(circle,"perimeter",{
    get: function(){
        return this.r*2*Math.PI;
    },
    set: function(val){
        this.r=val/(Math.PI*2);
    }
})
Object.getOwnPropertyDescriptor(circle,"perimeter"); // {enumerable: false, configurable: false,set: function()..,get: function()...}

//还原r值
circle.r = 10;
console.log(circle.perimeter); // 62.83....
circle.perimeter=62800;
console.log(circle.r); // 9994.93...



```

## Object.defineProperties() <ES5> IE9+
用法同`Object.defineProperty()`，但可以添加或修改一个或者多个自有属性

#### 语法

```js
Object.defineProperties(obj, props)
```
#### 参数

**obj**  
将要被添加属性或修改属性的对象  

**props**  
该对象的一个或多个键值对定义了将要为对象添加或修改的属性的具体配置（对象类型）


```js
var obj = {};
Object.defineProperties(obj, {
  "property1": {
    value: true,
    writable: true
  },
  "property2": {
    value: "Hello",
    writable: false
  }
  // 等等.
});
alert(obj.property2) //弹出"Hello"
```


**参考**：
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty
