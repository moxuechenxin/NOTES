[文章地址](http://www.zcfy.cc/article/elements-of-javascript-style-javascript-scene-medium-2769.html)

## 代码风格
**基本准则**：
- 让函数成为代码的基本单元。每个函数做一件事。
- 去掉无用的代码
- 使用主动语态
- 避免一连串松散结构的代码
- 把相关的代码放在一起。
- 表达式和陈述语句中使用主动语态。
- 用并行的代码表达并行的概念。

## 说明
### 让函数成为代码的基本单元。每个函数做一件事。
**JavaScript有三种类型的函数**：
- 交流型函数：执行I/O的函数
- 功能型函数：一系列指令的合集
- 映射型函数：给一些输入，返回相应的输出

**理想的函数应该是一个简单的，确定的，纯粹功能函数**：
- 给定相同的输入，返回相同的输出
- 没有副作用

### 去掉无用代码
#### 简化代码

```js
function secret (message) {
  return function () {
    return message;
  }
}
```
简化后：
```js
const secret = msg => () => msg;
```

#### 去掉无用的变量

```js
const getFullName = ({firstName, lastName}) => {
  const fullName = firstName + ' ' + lastName;
  return fullName;
};
```
优化后：
```js
const getFullName = ({firstName, lastName}) => (
  firstName + ' ' + lastName
);
```
**减少变量的另一个做法是**：利用`函数组合`以及`Point-free` 的风格

`Point-free` 风格是指：定义函数时无需引用对其操作的参数。
常用的point-free风格方式主要是`curry`和`函数组合`

```js
/* curry */

const add2 = a => b => a + b;

// Now we can define a point-free inc()
// that adds 1 to any number.
const inc = add2(1);

inc(3); // 4
```
函数组合是把一个函数结果应用到另一个函数的处理流程。
```js
/* 函数组合 */

const g = n => n + 1;
const f = n => n * 2;

// With points:
const incThenDoublePoints = n => {
  const incremented = g(n);
  return f(incremented);
};

incThenDoublePoints(20); // 42

// compose2 - Take two functions and return their composition
const compose2 = (f, g) => x => f(g(x));

// Point-free:
const incThenDoublePointFree = compose2(f, g);

incThenDoublePointFree(20); // 42
```

```js
/* 仿函数 */

const compose2 = (f, g) => x => [x].map(g).map(f).pop();

const incThenDoublePointFree = compose2(f, g);

incThenDoublePointFree(20); // 42
```

### 使用主动语态
#### 命名越直接越好
- `myFunction.wasCalled()`优于`myFunction.hasBeenCalled()`。
- `createUser()` 优于`User.create()`。
- `notify()`优于`Notifier.doNotification()`。

#### 命名断言或者布尔变量时尽量使用是或否的问题形式：
- `isActive(user)`优于`getActiveStatus(user)`。
- `isFirstRun = false;`优于`firstRun = false;`。

#### 命名函数使用动词形式
- `increment()`优于`plusOne()`。
- `unzip()`优于`filesFromZip()`。
- `filter(fn, array)`优于`matchingItemsFromArray(fn,array)`

#### 事件处理
事件处理函数和生命周期的函数是个例外，要避免使用动词形式，因为他们通常是为了说明这时该做什么而不是他们作为主语自身要做了什么。功能应该和命名一致
- `element.onClick(handleClick)`优于`element.click(handleClick)`。
- `component.onDragStart(handleDragStart)`优于`component.startDrag(handleDragStart)`。

### 避免一连串松散的语句
### 把相关的代码放在一起
### 陈述句和表达式使用主动语态
- `isFlying`优于`isNotFlying`
- `late`优于`notOnTime`

#### 尽量选择语气强烈的否定句
- `if (missingValue)`优于`if (!hasValue)`
- `if (anonymous)`优于`if (!user)`
- `if (isEmpty(thing))`优于`if (notDefined(thing))`

#### 函数调用时避免使用`null`和`undefined`参数类型

### 使用平行结构