## 观察者模式
```js
function EventTarget() {
    this.handlers = {};
}

EventTarget.prototype = {
    constructor: EventTarget,
    addHandler: function(type, handler) {
        this.handlers[type] = this.handlers[type] || [];
        this.handlers[type].push(handler);
    },
    fire: function(event) {
        event.target = event.target || this;
        if (this.handlers[event.type] instanceof Array) {
            var handlers = this.handlers[event.type];
            for (var i = 0, len = handlers.length; i < len; i++) {
                handlers[i](event);
            }
        }
    },
    removeHandler: function(type, handler) {
        if (this.handlers[type] instanceof Array) {
            var handlers = this.handlers[type];
            if(!handler){
                handlers.splice(0, handlers.length);
            }else{
                for (var i = 0, len = handlers.length; i < len; i++) {
                    if (handlers[i] === handler && handler) {
                        break;
                    }
                }
                handlers.splice(i, 1);
            }
            
        }
    }
};

/*使用*/
function handleMessage(event){
    alert("Message received: " + event.message);
}
//创建一个新对象
var target = new EventTarget();
//添加一个事件处理程序
target.addHandler("message", handleMessage);
//触发事件
target.fire({ type: "message", message: "Hello world!"});
//删除事件处理程序
target.removeHandler("message", handleMessage);
//再次，应没有处理程序
target.fire({ type: "message", message: "Hello world!"});

/*其他对象可以继承 EventTarget 并获得这个行为*/
function Person(name, age){
    EventTarget.call(this);
    this.name = name;
    this.age = age;
}
inheritPrototype(Person,EventTarget); //封装的原型继承函数
Person.prototype.say = function(message){
this.fire({type: "message", message: message});
};
function handleMessage(event){
alert(event.target.name + " says: " + event.message);
}
//创建新 person
var person = new Person("Nicholas", 29);
//添加一个事件处理程序
person.addHandler("message", handleMessage);
//在该对象上调用 1 个方法，它触发消息事件
person.say("Hi there.");
```
## DOM对象自定义事件

```js
function bindHandler(obj,type,handler){
    obj.handlers=obj.handlers || {};
    var handlerArr=obj.handlers[type]=obj.handlers[type] || [];
    handlerArr.push( handler.bind(obj) );
    if(obj.addEventListener){
		obj.addEventListener(type,handler,false);
	}
	else{
		obj.attachEvent('on'+type,handler);
	}
}
function fireHandler(obj,type){
    var handlerArr=obj.handlers[type];
    if( handlerArr instanceof Array){
        for(var i=0;i<handlerArr.length;i++){
            handlerArr[i]();
        }
    }
}
function removeHandler(obj,type,handler){
    var handlerArr=obj.handlers[type];
    if( handlerArr instanceof Array){
        if(!handler){
            handlerArr.splice(0,handlerArr.length);
        }else{
            for(var i=0;i<handlerArr.length;i++){
                if(handlerArr[i] === handler){
                    break;
                }
            }
            handlerArr.splice(i, 1);
        }
        
    }
}
```
