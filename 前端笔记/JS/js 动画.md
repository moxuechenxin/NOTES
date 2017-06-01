## requestAnimationFrame()
（IE10+）用来在页面重绘之前，通知浏览器调用一个指定的函数，以满足开发者操作动画的需求。这个方法接受一个函数为参，该函数会在重绘前调用。（返回requestId）

**语法**：
```js
requestID = window.requestAnimationFrame(callback);               // Firefox 23 / IE10 / Chrome / Safari 7 (incl. iOS)
requestID = window.mozRequestAnimationFrame(callback);                // Firefox < 23
requestID = window.webkitRequestAnimationFrame(callback); // Older versions Chrome/Webkit
```

**参数**：
- **callback**  
在每次需要重新绘制动画时,会调用这个参数所指定的函数。这个回调函数会收到一个参数，这个 DOMHighResTimeStamp 类型的参数指示当前时间距离开始触发 requestAnimationFrame 的回调的时间

**注意**：  
- **如果想得到连贯的逐帧动画，函数中必须重新调用 requestAnimationFrame()**
- 回调函数只会被传入一个`DOMHighResTimeStamp`参数，这个参数指示当前被 `requestAnimationFrame` 序列化的函数队列被触发的时间。因为很多个函数在这一帧被执行，所以每个函数都将被传入一个相同的时间戳，尽管经过了之前很多的计算工作。这个数值是一个小数，单位毫秒，精确度在 10 µs

```js
window.requestAnimationFrame = window.requestAnimationFrame || window.mozRequestAnimationFrame || window.webkitRequestAnimationFrame || window.msRequestAnimationFrame;

var start = null;
var d = document.getElementById('SomeElementYouWantToAnimate');

function step(timestamp) {
  if (start === null) start = timestamp;
  var progress = timestamp - start;
  d.style.left = Math.min(progress/10, 200) + "px";
  if (progress < 2000) {
    requestAnimationFrame(step);
  }
}
requestAnimationFrame(step);
```

### window.cancelAnimationFrame(requestId)
取消这个回调函数

**Ployfill**
```js
(function() {
    var lastTime = 0;
    var vendors = ['webkit', 'moz'];
    for(var x = 0; x < vendors.length && !window.requestAnimationFrame; ++x) {
        window.requestAnimationFrame = window[vendors[x] + 'RequestAnimationFrame'];
        window.cancelAnimationFrame = window[vendors[x] + 'CancelAnimationFrame'] ||    // Webkit中此取消方法的名字变了
                                      window[vendors[x] + 'CancelRequestAnimationFrame'];
    }

    if (!window.requestAnimationFrame) {
        window.requestAnimationFrame = function(callback, element) {
            var currTime = new Date().getTime();
            var timeToCall = Math.max(0, 16.7 - (currTime - lastTime));
            var id = window.setTimeout(function() {
                callback(currTime + timeToCall);
            }, timeToCall);
            lastTime = currTime + timeToCall;
            return id;
        };
    }
    if (!window.cancelAnimationFrame) {
        window.cancelAnimationFrame = function(id) {
            clearTimeout(id);
        };
    }
}());
```


**参考**：
- [张鑫旭：requestAnimationFrame](http://www.zhangxinxu.com/wordpress/2013/09/css3-animation-requestanimationframe-tween-%E5%8A%A8%E7%94%BB%E7%AE%97%E6%B3%95/)
- [MDN：requestAnimationFrame](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame)
