## 区别
**break**：
- break 可用于跳出**switch语**句和**循环语句**  

**continue**
- continue 只能用于**循环语句**。只是退出当前循环，根据控制表达式还允许继续进行下一次循环

## 用法
### break

```js
for (i=0;i<5;i++){
    if (i==3) break;
    x=x + "The number is " + i + "<br>";
}

/*
    The number is 0
    The number is 1
    The number is 2
*/
```

### continue

```js
for (i=0;i<=5;i++){
    if (i==3) continue;
    x=x + "The number is " + i + "<br>";
}
/*
    The number is 0
    The number is 1
    The number is 2
    The number is 4
    The number is 5
*/
```

### js标签

```js
var cars=[0,1,2,3];
list:
{
    console.log(cars[0]);
    console.log(cars[1]);
    break list;
    console.log(cars[2]);
    console.log(cars[3]);
}

// 0
// 1
```

**与break语句一起使用**

```js
var iNum = 0;

outermost:
for (var i=0; i<10; i++) {
  for (var j=0; j<10; j++) {
    if (i == 5 && j == 5) {
        break outermost;
    }
    iNum++;
  }
}

alert(iNum);	//输出 "55"
```

**与continue语句一起使用**

```js
var iNum = 0;

outermost:
for (var i=0; i<10; i++) {
  for (var j=0; j<10; j++) {
    if (i == 5 && j == 5) {
        continue outermost;
    }
    iNum++;
  }
}

alert(iNum);	//输出 "95"

```






**参考**：
- http://www.w3school.com.cn/js/pro_js_statements_labeled.asp
- http://www.w3school.com.cn/js/pro_js_statements_break_continue.asp
- http://www.w3school.com.cn/js/js_break.asp