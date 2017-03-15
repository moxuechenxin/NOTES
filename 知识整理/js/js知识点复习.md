## AJAX
```js
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function(){
    if(xhr.readyState === 4){
        if(xhr.status === 200){
            
        }
    }
}
xhr.open("GET",url+,true);
xhr.setRequestHeader("content-type","");
xhr.send(null);

xhr.open("POST",url,true);
xhr.setRequestHeader("content-type","application/x-www-form-urlencoded")
xhr.send(data);
```
## 继承

```js
function Fn1(opts){
    this.name = opts.name;
}
Fn1.prototype = {
    constructor: Fn1,
    sayHi:function(){
        console.log('hi','my name is '+ this.name);
    }
}

function Fn2(opts){
    Fn1.call(this,opts);
    this.age = opts.age;
}
Fn2.prototype = Object.create(Fn1.prototype);
Fn2.prototype.constructor = Fn2;
Fn2.prototype.introduce = function(){
    this.sayHi();
    console.log('my age is '+ this.age);
}

```
## 