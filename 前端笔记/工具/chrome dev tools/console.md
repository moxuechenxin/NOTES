### console.log家族
- **console.log** ('普通信息')  
\- console.log() 接收不定参数，参数间用逗号分隔，最终会输出会将它们以空白字符连接
- **console.info** ('提示性信息')
- **console.error** ('错误信息')
- **console.warn** ('警示信息')

### console.dir()
\- 显示一个对象所有的属性和方法

### console.dirxml()
\- 显示网页的某个节点（node）所包含的html/xml代码

### console.trace()
\- 用来追踪函数的调用轨迹

```js
/*函数是如何被调用的，在其中加入console.trace()方法就可以了*/
function add(a,b){
  console.trace();
　return a+b;
}
var x = add3(1,1);
function add3(a,b){return add2(a,b);}
function add2(a,b){return add1(a,b);}
function add1(a,b){return add(a,b);}
//依次输出 add() add1() add2() add3()以及各自的参数值
```

### console.time()和console.timeEnd()
\- 用来显示代码的运行时间

```js
console.time("控制台计时器一");
for(var i=0;i<1000;i++){
　　for(var j=0;j<1000;j++){}
}
console.timeEnd("控制台计时器一");
//输出：38.84ms
```

### console.group()和console.groupEnd()
\- 信息分组

```js
console.group("第一组信息");
　　　　console.log("第一组第一条:我的博客(http://www.ido321.com)");
　　　　console.log("第一组第二条:CSDN(http://blog.csdn.net/u011043843)");
console.groupEnd();
console.group("第二组信息");
　　　　console.log("第二组第一条:程序爱好者QQ群： 259280570");
　　　　console.log("第二组第二条:欢迎你加入");
console.groupEnd();
```

### 占位符
\- console上述的集中度支持printf的占位符格式，支持的占位符有：字符（%s）、整数（%d或%i）、浮点数（%f）和对象（%o）

```js
 console.log("%d年%d月%d日",2011,3,26);  // 2011年3月26日
```
### console.table()
\- 输出表格
```js
var data = {code:200,content:[{'品名': '杜雷斯', '数量': 4}, {'品名': '冈本', '数量': 3}]};
console.table(data.content);
```
### console.count()
\- 计数
```js
function fun(){
    console.count("fun被执行的次数");
}
fun();
fun();
fun();
// fun被执行的次数：1
// fun被执行的次数：2
// fun被执行的次数：3

```

### console.profile()
\- 性能分析（Profiler）就是分析程序各个部分的运行时间，找出瓶颈所在

```js
function All(){
    alert(11);
　　for(var i=0;i<10;i++){
        funcA(1000);
    }
　　    funcB(10000);
}

function funcA(count){
　　for(var i=0;i<count;i++){}
}

function funcB(count){
　　for(var i=0;i<count;i++){}
}

console.profile('性能分析器');
All();
console.profileEnd();
//输出结果在Profiles面板查看
```

### $
#### $0~$4表示最近5个选择过的DOM节点
#### 类似jQuery的选择器用法:
`$('div')` 等价于 `document.querySelector('div')`  
`$$('div')` 等价于 `document.querySelectorAll('div')`

### $x(path)
\- 将所匹配的节点放在一个数组里返回
```js
$x("//p");  // 匹配所有的p元素
$x("//p[a]"); // 匹配所有子节点包含a的p元素
```

### inspect(el / function)
\- 检查DOM中的元素或函数(类似元素审查)

### $_
\- 重新获取上一次结果的值

### keys & values
\- keys(obj) 返回传入对象所有属性名组成的数据  
\- values(obj) 返回所有属性值组成的数组
```js
var tfboy={name:'wayou',gender:'unknown',hobby:'opposite to the gender'};
keys(tfboy); // ["name","gender","hobby"]
values(tfboy); // ["wayou","unknown","opposite to the gender"]

```
### monitor & unmonitor
\- monitor(function):接收一个函数名作为参数，比如function a，每次a被执行了，都会在控制台输出一条信息，里面包含了函数的名称a及执行时所传入的参数  
\- unmonitor(function)便是用来停止这一监听
```js
function sayHello(name){
    alert('hello,'+name);
}
monitor(sayHello);
sayHello('damonare');
sayHello('tjz');
unmonitor(sayHello);

```

### getEventListeners(el)
\- 查看元素绑定的事件处理函数  

### monitorEvents(el [,event])
\- 监控元素上发生的事件，并通过`console.log(e.type,e)`打印结果
- monitorEvents(el)  
监控元素上所关联的所有事件
- monitorEvents(el,'click')  
监控元素上的click事件
- monitorEvents(el,['click','mouseover'])  
监控元素上的click和mouseover事件




参考：  
https://github.com/dwqs/blog/issues/32    
http://web.jobbole.com/87969/
