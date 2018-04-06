## 用途
- 生成网页截图或者 PDF 
- 高级爬虫，可以爬取大量异步渲染内容的网页 
- 模拟键盘输入、表单自动提交、登录网页等，实现 UI 自动化测试 
- 捕获站点的时间线，以便追踪你的网站，帮助分析网站性能问题

## 基本API
### puppeteer.launch([options])
返回一个promise，使用then方法获取browser实例

**options参数**：

参数名称	| 参数类型	| 参数说明
---|---|---
`ignoreHTTPSErrors`	| Boolean	| 默认`false`，在请求的过程中是否忽略 Https 报错信息
`headless`	| Boolean	| 默认`true`，是否以”无头”的模式运行 chrome, 也就是不显示 UI
`executablePath`	| String	| 可执行文件(Chromium或Chrome)的路径，默认是install的Chromium，相对路径是相对当前工作目录
`slowMo`	| Number	| 使 Puppeteer 操作减速，单位是毫秒。如果你想看看 Puppeteer 的整个工作过程，这个参数将非常有用
`args`	| Array(String)	| 传递给 chrome 实例的[其他参数](https://peter.sh/experiments/chromium-command-line-switches/)
`ignoreDefaultArgs` | Boolean | 默认`false`，不使用`puppeteer.defaultArgs()`，危险的选项
`handleSIGINT`	| Boolean	| 默认`true`，是否可以使用`CTRL+C`关闭并退出浏览器
`handleSIGTERM` | Boolean | 默认`true`，是否可以使用`SIGTERM`关闭并退出浏览器
`handleSIGHUP` | Boolean | 默认`true`，是否可以使用`SIGHUP`关闭并退出浏览器
`timeout`	| Number	| 默认30000（30秒），等待 Chrome 实例启动的最长时间，如果传入 0 的话则不限制时间
`dumpio`	| Boolean	| 默认`false`，是否将浏览器进程`stdout`和`stderr`导入到`process.stdout`和`process.stderr`中
`userDataDir`	| String	| 设置用户数据目录，默认linux 是在 ~/.config 目录，window 默认在 C:\Users{USER}\AppData\Local\Google\Chrome\User Data, 其中 {USER} 代表当前登录的用户名
`env`	| Object	| 默认为`process.env`，指定对Chromium可见的环境变量
`devtools`	| Boolean	| 是否为每个选项卡自动打开DevTools面板，如果为true，则`headless`会被设置为`false`
`pipe` | Boolean | 默认`false`，是否通过一个`pipe`而不是`WebSocket`与浏览器连接

### browser对象
当Puppeteer连接到一个Chrome实例的时候就会创建一个 Browser 对象，有以下两种方式：
- `puppeteer.launch `
- `puppeteer.connect`

### page对象
#### 获取元素
- `page.$(selector)` = `document.querySelector(selector)`
- `page.$$(selector)` = `document.querySelectorAll(selector)`

返回一个Promise，传入then中的值为`ElementHandle`

```js
let inputElement = await page.$("#search", input => input);
//下面写法等价
let inputElement = await page.$('#search');

const links = await page.$$("a");
//下面写法等价
const links = await page.$$("a", links => links);
```

#### 获取元素属性
- `Page.$eval(selector, pageFunction[, …args])`
- `Page.$$eval(selector, pageFunction[, …args])`

`…args`：传给`pageFunction`的参数

该方法返回一个Promise，传入then中的值为`pageFunction`调用的返回值

```js
const searchValue = await page.$eval('#search', el => el.value);

const divsCounts = await page.$$eval('div', divs => divs.length);
```

#### 执行自定义的js脚本
- `page.evaluate(pageFunction|string, …args)`
该方法返回一个Promise，传入then中的值为`pageFunction`调用的返回值(可序列化对象)

```js
const result = await page.evaluate(x => {
  return Promise.resolve(8 * x);
}, 7);
console.log(result); // prints "56"

console.log(await page.evaluate('1 + 2')); // prints "3"
const x = 10;
console.log(await page.evaluate(`1 + ${x}`)); // prints "11"
```

- `page.evaluateHandle(pageFunction, ...args)`
该方法返回一个Promise，传入then中的值为`pageFunction`调用的返回值(JSHandle对象)

- `page.evaluateOnNewDocument(pageFunction, ...args)`
在文档被创建后，所有脚本执行之前，调用`pageFunction`, 一般是用来初始化 javascript 环境的，比如重置或者 初始化一些全局变量

- `page.exposeFunction(name, puppeteerFunction)`
在页面注册全局函数(挂载到window对象上)

```js
// 添加一个md5方法到page上下文

const puppeteer = require('puppeteer');
const crypto = require('crypto');

puppeteer.launch().then(async browser => {
  const page = await browser.newPage();
  page.on('console', msg => console.log(msg.text()));
  await page.exposeFunction('md5', text =>
    crypto.createHash('md5').update(text).digest('hex')
  );
  await page.evaluate(async () => {
    // use window.md5 to compute hashes
    const myString = 'PUPPETEER';
    const myHash = await window.md5(myString);
    console.log(`md5 of ${myString} is ${myHash}`);
  });
  await browser.close();
});
```

## 模拟器(客户端)运行配置
- `page.emulate(options)`
**options**:
  - `viewport`：Object
    - `width`
    - `height`
    - `deviceScaleFactor`：默认为`1`，dpr
    - `isMobile`：默认为`false`
    - `hasTouch`：默认为`false`
    - `isLandscape`：默认为`false`，指定视口是否处于横向模式
  - `userAgent`：String

有两个快捷方法：
- `page.setViewport(viewport)`
- `page.setUserAgent(userAgent)`

`puppeteer/DeviceDescriptors`模块包含了一些设备的配置(主要是移动设备)

```js
const puppeteer = require('puppeteer');
const devices = require('puppeteer/DeviceDescriptors');
const iPhone = devices['iPhone 6'];

puppeteer.launch().then(async browser => {
  const page = await browser.newPage();
  await page.emulate(iPhone);
  await page.goto('https://www.google.com');
  // other actions...
  await browser.close();
});
```

## 键盘和鼠标
### 键盘
- `keyboard.down(key[, options])`  
触发keydown事件，key表示键的名称，详细的键名映射[戳这里](https://github.com/GoogleChrome/puppeteer/blob/master/lib/USKeyboardLayout.js)
- `keyboard.up(key[, options])`  
触发keyup事件
- `keyboard.press(key[, options])`  
按下某个键
- `keyboard.sendCharacter(char)`  
输入一个字符
- `keyboard.type(text, options)`  
输入一个字符串

```js
page.keyboard.press("Shift"); //按下 Shift 键
page.keyboard.sendCharacter('嗨');
page.keyboard.type('Hello'); // 一次输入完成
page.keyboard.type('World', {delay: 100}); // 像用户一样慢慢输入
```

### 鼠标
- `mouse.click(x, y, [options])`  
移动鼠标指针到指定的位置，然后按下鼠标（是 `mouse.move` 和 `mouse.down` 或 `mouse.up` 的快捷操作）

  `options`: 
    - `button`：默认`left`，可选值为`left`、`right`、`middle`
    - `clickCount`：默认`1`，点击次数
    - `delay`：默认`0`，在`mousedown`和`mouseup`之间的延迟时间
  
- `mouse.down([options])`  
触发 mousedown 事件

  `options`: 
    - `button`：默认`left`，可选值为`left`、`right`、`middle`
    - `clickCount`：默认`1`，点击次数

- `mouse.up([options])`  
触发mouseup事件

  `options`: 
    - `button`：默认`left`，可选值为`left`、`right`、`middle`
    - `clickCount`：默认`1`，点击次数


- `mouse.move(x, y, [options])`  
移动鼠标到指定位置，`options.steps` 表示移动的步长，默认为1

## 其他API
### page.waitFor系列
返回一个Promise，then中传的值为一个`JSHandle`

- `page.waitFor(selectorOrFunctionOrTimeout[, options[, ...args]])`

**下面是快捷方法**:
- `page.waitForSelector(selector[, options])`  
等待某个选择器的元素加载完成之后，这个元素可以是异步加载的

- `page.waitForFunction(pageFunction[, options[, ...args]])`  
等待pageFunction执行完成之后

- `page.waitForNavigation(options)`  
等待页面基本元素加载完之后，比如同步的HTML, CSS, JS 等代码



- - -
- [官方api文档](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md)
- [Puppeteer 入门教程](http://www.r9it.com/20171106/puppeteer.html)
- [无头浏览器 Puppeteer 初探](https://github.com/ProtoTeam/blog/blob/master/201710/2.md)

