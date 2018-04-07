## async函数对 Generator 函数的改进
- **内置执行器**
- **更好的语义**
- **更广的适用性**  
sync函数的await命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作）

- **返回值是Promise**

## 基本用法
async函数返回一个 Promise 对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句

**async函数的使用形式**：
```js
// 函数声明
async function foo() {}

// 函数表达式
const foo = async function () {};

// 对象的方法
let obj = { async foo() {} };
obj.foo().then(...)

// Class 的方法
class Storage {
  constructor() {
    this.cachePromise = caches.open('avatars');
  }

  async getAvatar(name) {
    const cache = await this.cachePromise;
    return cache.match(`/avatars/${name}.jpg`);
  }
}

const storage = new Storage();
storage.getAvatar('jake').then(…);

// 箭头函数
const foo = async () => {};
```

## 语法
### 返回 Promise 对象
`async`函数返回一个`Promise`对象
`async`函数内部`return`语句返回的值，会成为`then`方法回调函数的参数  
`async`函数内部抛出错误，会导致返回的 `Promise` 对象变为`reject`状态。抛出的错误对象会被`catch`方法回调函数接收到

```js
/* then */
async function f() {
  return 'hello world';
}

f().then(v => console.log(v)) // "hello world"

/* catch */
async function f() {
  throw new Error('出错了')
}

f().catch(e => console.log(e)) // Error: 出错了
```

### Promise 对象的状态变化
`async`函数返回的 `Promise` 对象，必须等到内部所有`await`命令后面的 `Promise` 对象执行完，才会发生状态改变，除非遇到`return`语句或者抛出错误。也就是说，只有`async`函数内部的异步操作执行完，才会执行`then`方法指定的回调函数

### await命令
正常情况下，`await`命令后面是一个`Promise`对象。如果不是，会被转成一个立即`resolve`的`Promise`对象  
`await`命令后面的`Promise`对象如果变为`reject`状态，则`reject`的参数会被`catch`方法的回调函数接收到（只要一个`await`语句后面的`Promise`变为`reject`，那么整个`async`函数都会中断执行）

### 错误处理

### 使用注意点
1. `await`命令后面的`Promise`对象，运行结果可能是`rejected`，所以最好把`await`命令放在`try...catch`代码块中

2. 多个`await`命令后面的异步操作，如果不存在继发关系，最好让它们同时触发
```js
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```

3. `await`命令只能用在`async`函数之中
```js
async function dbFuc(db) {
  let docs = [{}, {}, {}];

  // 报错
  docs.forEach(function (doc) {
    await db.post(doc);
  });
}
```
