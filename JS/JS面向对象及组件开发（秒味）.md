## 1. 面向对象

```js
//构造函数
function Fn(){
    this.age=11;
}
//原型上设置方法
Fn.prototype.hi=function(){
    alert("HELLO");
}
var fn1=new Fn();
fn1.hi();
```
### 1.1 面向对象的属性和方法
- **hasOwnProperty(attr)**: 看是不是对象自身下面的属性
- **constructor**: 查看对象的构造函数  
\- 每个原型都会自动添加constructor属性  
\- For in 的时候有些属性是找不到的  
\- 避免修改construtor属性  
- **instanceof运算符**  
\- 对象与构造函数在原型链上是否有关系
- **toString()**:  object上的方法  
\- 系统对象下toString()都是自带的，自己写的对象都是通过原型链找到的  
## 2. 继承

```js
// 属性继承：调用父类的构造函数 call
/*** 方法继承：
   * 1. for in :拷贝继承(jquery也是采用拷贝继承extend)
   * 2. 类式继承 ：利用构造函数(类)继承的方式
   * 3. 原型继承 ：借助原型来实现对象继承对象
   */
function Parent(name,age){
    this.name=name;
    this.age=age;
}
Parent.prototype.sayHi=function(){
    alert("Hi,I'm"+this.name);
}
function Child(name,age,job){
    //属性继承
    Parent.call(this,name,age);
}
// 1. 拷贝继承
function extend(obj1,obj2){
    for(var attr in obj2){
        obj1[attr]=obj2[attr];
    }
}
extend(Child.prototype,Person.prototype);
// 2. 类式继承
// 注意：constructor指向需要修正
// 注意：若直接new parent()赋值给Child.prototype，则方法和属性一并继承，一旦继承的属性为引用类型，所以继承后的实例属性均共用同一个，所有需要修正
// 以下代码只能继承方法
function F(){};
F.prototype=Parent.prototype;
Child.prototype=new F(); // 以上三行代码等同于Child.prototype=Object.create(Parent.prototype); 创建一个空对象，空对象的原型为Parent.prototype（ES5方法）
Child.prototype.constructor=Child;
// 3. 原型继承
// 与类式继承类似
// 以下函数等同于Object.create() (ES5)
function createObject(obj){
    function F(){}
    F.prototype=obj;
    return new F();
}
```
## 3. 组件开发
### 3.1 什么是组件？
- 对面向对象的深入应用（UI组件，功能组件）
- 将 配置参数、方法、事件，三者进行分离
### 3.2 自定义事件

