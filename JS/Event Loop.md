## NodeJS中的Event Loop
### 当Node.js启动时
1. 初始化Event Loop
2. 开始执行脚本(或者进入REPL)，这些脚本可能会调用一些异步API、设定定时器或调用`process.nextTick()`
3. 开始处理Event Loop

### Event Loop的处理过程
```
   ┌───────────────────────┐
┌─>│        timers         │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     I/O callbacks     │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     idle, prepare     │
│  └──────────┬────────────┘      ┌───────────────┐
│  ┌──────────┴────────────┐      │   incoming:   │
│  │         poll          │<─────┤  connections, │
│  └──────────┬────────────┘      │   data, etc.  │
│  ┌──────────┴────────────┐      └───────────────┘
│  │        check          │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
└──┤    close callbacks    │
   └───────────────────────┘
```
每个阶段都有一个「先入先出队列」，这个队列存有要执行的回调函数（函数地址）。一般来说，当Event Loop达到某个阶段时，会在这个阶段进行一些特殊的操作，然后执行这个阶段的队列中的所有回调，在以下两种情况下停止执行：
- 队列的操作全部执行完毕
- 执行的回调数目达到指定的最大值，然后，Event Loop进入下一个阶段

**node中的事件循环的顺序**：  

外部输入数据-->轮询阶段(poll)-->检查阶段(check)-->关闭事件回调阶段(close callback)-->定时器检测阶段(timer)-->I/O事件回调阶段(I/O callbacks)-->闲置阶段(idle, prepare)-->轮询阶段...

### 各阶段概览
阶段 | 概览
---|---
timers | 执行`setTimeout`和`setInterval`的回调函数
I/O callbacks | 执行不在 timers 阶段、close callbacks 阶段和 check 阶段的回调
idle, prepare | Event Loop内部使用的阶段
poll | 获取新的I/O事件（在某些场景下Node会阻塞在这个阶段）
check | 执行`setImmediate`的回调函数
close callbacks | 执行关闭事件的回调函数，如`socket.on('close', callback)`

一个 Node.js 程序结束时，会检查Event Loop是否在等待异步 I/O 操作结束，是否在等待计时器触发，如果没有，就会关掉Event Loop

### 各阶段详解
#### timers阶段
计时器实际上是在指定多久以后可以执行某个回调函数

#### I/O callbacks
这个阶段会执行一些系统操作的回调函数（比如 TCP 报错，如果一个 TCP socket 开始连接时出现了 ECONNREFUSED 错误，一些 *nix 系统就会（向 Node.js）通知这个错误。这个通知就会被放入 I/O callbacks 队列）

#### poll阶段（轮询阶段）
poll阶段有两个功能：
- 如果发现计时器的时间到了，就绕回到timers阶段执行计时器的回调
- 然后，执行poll队列中的回调

当Event Loop进入poll阶段，如果发现没有计时器，就会：
- 如果poll队列不是空的，依次执行队列中的回调函数，直到队列被清空或者达到poll阶段的时间上限
- 如果poll队列是空的：
  1. 如果有`setImmediate`任务，Event Loop就结束poll阶段去check阶段
  2. 如果没有，则等待新的回调函数进入poll队列，并立即执行它

一旦 poll 队列为空，event loop 就会检查计时器有没有到期，如果有计时器到期了，event loop 就会回到 timers 阶段执行计时器的回调

#### check阶段
这个阶段允许开发者在 poll 阶段结束后立即执行一些函数。如果 poll 阶段空闲了，同时存在`setImmediate`任务，Event Loop 就会进入 check 阶段

#### close callbacks阶段
如果一个 socket 或者 handle 被突然关闭（比如`socket.destroy()`），那么就会有一个 close 事件进入这个阶段

### process.nextTick()
不管Event Loop当前处于哪个阶段，nextTick队列都是在当前阶段后下一阶段前就被执行了

### setTimeout 和 setImmediate
**情况一**：如果 setTimeout 和 setImmediate 都是在主模块（main module）中被调用的，那么回调的执行顺序取决于当前进程的性能，这个性能受其他应用程序进程的影响，即两个回调的执行顺序是无法判断
```js
setTimeout(() => {
  console.log('timeout');
}, 0);

setImmediate(() => {
  console.log('immediate');
});
// 执行顺序不固定
```

**情况二**：如果放到 I/O 操作的回调里，setImmediate 的回调就总是优先于 setTimeout 的回调
```js
const fs = require('fs');

fs.readFile(__filename, () => {
  setTimeout(() => {
    console.log('timeout');
  }, 0);
  setImmediate(() => {
    console.log('immediate');
  });
});
// immediate
// timeout
```

问题的关键在于setTimeout何时到期，只有到期的setTimeout才能保证在setImmediate之前执行

## 浏览器中的Event Loop
### 几个概念
- 执行栈
- 事件队列：宏任务队列/微任务队列

### Event Loop流程
1. 执行当前`执行栈`中的任务
2. 异步事件返回结果后，将该事件加入到`事件队列`中
3. 等待当前执行栈中的所有任务执行完毕，主线程处于闲置状态时，将`事件队列`中的任务按先后顺序放入`执行栈`中，然后执行其中的同步代码
4. 如此循环...

**注**：`事件队列`又分为两个队列：`MicroTask`队列和`MacroTask`队列，在上面第3步时先处理`MicroTask`队列，然后`MacroTask`队列

**`MicroTask`**： 包含了`process.nextTick`, `Promise`, `Object.observe`, `MutationObserver` 等    
**`MacroTask`**： 包含了`setTimeout`, `setInterval`, `setImmediate`, `requestAnimationFrame`, `I/O`, `UI rendering` 

**注**：同一次事件循环中，微任务永远在宏任务之前执行








- - - 
**参考**：
- [详解JavaScript中的Event Loop（事件循环）机制](https://zhuanlan.zhihu.com/p/33058983)
- [JavaScript Event Loop 机制详解与 Vue.js 中实践应用](https://zhuanlan.zhihu.com/p/29116364)
- [Event Loop的规范和实现](https://github.com/ProtoTeam/blog/blob/master/201801/2.md)
- [The Node.js Event Loop, Timers, and process.nextTick()](https://juejin.im/post/5ab7677f6fb9a028d56711d0)
- [并发模型与事件循环](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/EventLoop)
