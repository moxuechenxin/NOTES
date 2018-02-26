## 说明
将一个JavaScript代码字符串求值成特定的对象

## 语法
`eval(string)`

## 返回值
eval 将会返回对最后一个表达式的求值结果

## 用法
- eval()的参数是一个字符串。如果字符串表示了一个表达式，eval()会对表达式求值。如果参数表示了一个或多个JavaScript声明， 那么eval()会执行声明。可用于解析JSON（极不推荐）
- 如果参数不是字符串，eval()将会将参数原封不动的返回。

```js
var obj = {
    age : 18
}

eval('obj.age')  // 18 等同于eval('obj["age"]')


```

```js
var x=4;
var str = "if (x == 5) {alert('z is 42'); z = 42;} else z = 0; ";
eval(str); // 0

```



## 直接调用和间接调用的区别
**间接使用**：目标字符串中的 javascript 代码将被直接视为在**全局作用域**下执行

```js
function test() {
  var x = 2, y = 4;
  console.log(eval("x + y"));  // Direct call, uses local scope, result is 6
  var geval = eval;
  console.log(geval("x + y")); // Indirect call, uses global scope, throws ReferenceError because `x` is undefined
}
test();
```

## 错误信息
- 如果参数中没有合法的表达式和语句，则抛出 SyntaxError 异常。
- 如果非法调用 eval()，则抛出 EvalError 异常。
- 如果传递给 eval() 的 Javascript 代码生成了一个异常，eval() 将把该异常传递给调用者。

**参考**：
- http://www.w3school.com.cn/jsref/jsref_eval.asp
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval