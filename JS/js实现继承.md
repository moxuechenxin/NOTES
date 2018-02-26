## JS继承的实现方式
#### 继承原则：  
- **构造函数中设置的属性**：调用父类的构造函数 call (借用构造函数)
- **原型上的方法**：通过以下方法继承  

既然要实现继承，那么首先我们得有一个父类，代码如下：

```js
// 定义一个父类
function Parent(name) {
  // 属性
  this.name = name;
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
}
// 原型方法
Parent.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};

//子类
function Child(name,age){
  Parent.call(this);
  this.age=age;
}
```

### 1. 拷贝继承

```js
function extend(obj1,obj2){
    for(var attr in obj2){
        obj1[attr]=obj2[attr];
    }
}
extend(Child.prototype,Parent.prototype);

// Test Code
var child = new Child("paul");
console.log(child.name);
console.log(child.sleep());
console.log(child instanceof Parent); // false
console.log(child instanceof Child); // true
```
**特点**：  
　　1.支持多继承  
**缺点**：  
　　1.效率较低，内存占用高（因为要拷贝父类的属性）  
　　2.无法获取父类不可枚举的方法（不可枚举方法，不能使用for in 访问到）  
### 2.原型链继承
**核心**： 将父类的实例作为子类的原型
```js
Child.prototype=new Parent();
Child.prototype.constructor=Child;

//　Test Code
var child = new Child("paul");
console.log(child.eat("apple"));
```
**特点**：  
　　1.非常纯粹的继承关系，实例是子类的实例，也是父类的实例  
　　2.父类新增原型方法/原型属性，子类都能访问到  
　　3.简单，易于实现  
**缺点**：  
　　1.要想为子类新增属性和方法，必须要在new Parent()这样的语句之后执行，不能放到构造器中  
　　2.无法实现多继承  
　　3.来自原型对象的引用属性是所有实例共享的  
　　4.创建子类实例时，无法向父类构造函数传参  
### 3.原型式继承
**核心**： 借助原型可以基于已有的对象创建新对象，同时还不必须因此创建自定义的类型。
```js
// ES5 通过新增Object.create()方法规范化了原型式继承，这个方法接收两个参数：一个用作新对象原型的对象和一个作为新对象定义额外属性的对象
// https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create
function createObject(obj) {
    function F(){}
    F.prototype = obj;
    return new F();
}

Child.prototype=createObject(Parent.prototype);
//　Test Code
var child = new Child("paul");
console.log(child.eat("apple"));
```
### 4.寄生式继承
**核心**：创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后再像真正是它做了所有工作一样返回对象

```js
function createAnother(original){
  var clone = createObject(original); //通过调用函数创建一个新对象
  clone.sayHi = function(){ //以某种方式来增强这个对象
  alert("hi");
  };
  return clone; //返回这个对象
}
var person = {
  name: "Nicholas",
  friends: ["Shelby", "Court", "Van"]
};
var anotherPerson = createAnother(person);
anotherPerson.sayHi(); //"hi"
```
**特点**：  
　　1.新对象不仅具有 person
的所有属性和方法，而且还有自己的 sayHi() 方法  
　　2.在主要考虑对象而不是自定义类型和构造函数的情况下，寄生式继承也是一种有用的模式,任何能够返回新对象的函数都适用于此模式  
**缺点**：  
　　1.使用寄生式继承来为对象添加函数，会由于不能做到函数复用而降低效率
### 5.寄生组合式继承
**核心**：通过借用函数来继承属性，通过原型链的混成形式来继承方法

```js
function inheritProperty(child, parent) {
    // createObject函数见“原型式继承”
    var prototype = createObject(parent.prototype);//创建对象
    prototype.constructor = child;//增强对象
    child.prototype = prototype;//指定对象
}
inheritProperty(Child,Parent);
```
## 推荐写法
```js
function Parent(name,age){
    this.name=name;
    this.age=age;
}
Parent.prototype.sayhi=function(){
    console.log("hello");
}
function Child(name,age,array){
    Parent.call(this,name,age);
    this.array=[1,2,3];
}
//模拟Object.create()
/*
if( !Object.create ){
    Object.create=function(proto){
        function F(){};
        F.prototype=proto;
        return new F(); // 使用new操作符，如果不传参可省略括号
    }
}
*/
Child.prototype=Object.create(fn1.prototype); //ES5方法
Child.prototype.constructor=Child;

var obj1=new Child("john",20);
var obj2=new Child("mary",30);
console.log( obj1.name ); // "john"
console.log(obj1.sayhi()); // "hello"
obj1.array.push(4);
console.log(obj1.array); // [1,2,3,4]
console.log(obj2.array); // [1,2,3]
```

**参考**：  
- http://www.cnblogs.com/humin/p/4556820.html  
- http://javapolo.iteye.com/blog/1996871  
- https://segmentfault.com/a/1190000004730936  
- 《高三 6.3》
