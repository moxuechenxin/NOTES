## 事件
### visibilitychange
（IE10+）浏览器标签页被隐藏或显示的时候会触发visibilitychange事件  

**注意**：   
需要加厂商前缀：`webkit` (chrome),`moz` (firefox),`ms` (IE)  
`msvisibilitychange`,`webkitvisibilitychange`,`mozvisibilitychange`

## 属性
### document.hiden
（只读）如果页面处于被用户认为是隐藏状态时返回true，否则返回false。

### document.visibilityState
文档可见性状态。可能的值：
- **visible** : 页面内容至少是部分可见。 在实际中，这意味着页面时非最小化窗口的前景选项卡。
- **hidden** : 页面内容对用户不可见。 在实际中，这意味着文档可以是一个后台标签，或是最小化窗口的一部分，或是在操作系统锁屏激活的状态下。
- **prerender** : 页面内容正在被预渲染且没有对用户是不可见的(被当做隐藏，以 document.hidden 为目的 ). 文档可能初始状态为prerender，但绝不会从其它值转为该值。 注释：浏览器支持是可选的。
- **unloaded** : 页面正在从内存中卸载。 注释：浏览器支持是可选的。


```js
var pageVisibility = (function() {
    var prefixSupport, keyWithPrefix = function(prefix, key) {
        if (prefix !== "") {
            // 首字母大写
            return prefix + key.slice(0, 1).toUpperCase() + key.slice(1);
        }
        return key;
    };
    var isPageVisibilitySupport = (function() {
        var support = false;
        if (typeof window.screenX === "number") {
            ["webkit", "moz", "ms", "o", ""].forEach(function(prefix) {
                if (support == false && document[keyWithPrefix(prefix, "hidden")] != undefined) {
                    prefixSupport = prefix;
                    support = true;
                }
            });
        }
        return support;
    })();

    var isHidden = function() {
        if (isPageVisibilitySupport) {
            return document[keyWithPrefix(prefixSupport, "hidden")];
        }
        return undefined;
    };

    var visibilityState = function() {
        if (isPageVisibilitySupport) {
            return document[keyWithPrefix(prefixSupport, "visibilityState")];
        }
        return undefined;
    };

    return {
        hidden: isHidden(),
        visibilityState: visibilityState(),
        visibilitychange: function(fn, usecapture) {
            usecapture = undefined || false;
            if (isPageVisibilitySupport && typeof fn === "function") {
                return document.addEventListener(prefixSupport + "visibilitychange", function(evt) {
                    this.hidden = isHidden();
                    this.visibilityState = visibilityState();
                    fn.call(this, evt);
                }.bind(this), usecapture);
            }
            return undefined;
        }
    };
})(undefined);

pageVisibility.visibilitychange( function(e) {
    var body = document.querySelector("body");
    body.innerHTML += "<br/>hidden: " + this.hidden + "; visibilityState: " + this.visibilityState;
})
```

## 相关事件
### window.onblur/window.onfocus
可以检测当前页面是否处于活动状态

**参考**：
- https://developer.mozilla.org/zh-CN/docs/Web/API/Page_Visibility_API
- https://developer.mozilla.org/zh-CN/docs/Web/Events/visibilitychange
- http://www.zhangxinxu.com/wordpress/2012/11/page-visibility-api-introduction-extend/