## 说明
`navigator` 对象包含有关浏览器的信息

## 属性
属性 | 描述
---|---
cookieEnabled | 返回指明浏览器中是否启用 `cookie` 的布尔值
geolocation | 返回`geolocation`对象（IE9+）
userAgent | 返回由客户机发送服务器的 `user-agent` 头部的值
platform | 返回运行浏览器的操作系统平台
appCodeName | 返回浏览器的代码名
appMinorVersion	| 返回浏览器的次级版本
appName	| 返回浏览器的名称
appVersion | 返回浏览器的平台和版本信息
browserLanguage	| 返回当前浏览器的语言
cpuClass | 返回浏览器系统的 CPU 等级
onLine | 返回指明系统是否处于脱机模式的布尔值
systemLanguage | 返回 OS 使用的默认语言
userLanguage | 返回 OS 的自然语言设置

## [geolocation对象](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation/Using_geolocation)

**注意**：chrome默认不支持http协议调用以下访问，且不稳定，推荐使用第三方地图提供商api

### 属性


### 方法

#### [getCurrentPosition()](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation/getCurrentPosition)
异步地请求获取用户位置，并查询定位硬件来获取最新信息。当定位被确定后，定义的回调函数就会被执行

**语法**：

```js
navigator.geolocation.getCurrentPosition(success [, error] [, options])
```

**参数**：
- **success**：成功得到位置信息时的回调函数，使用`Position`对象作为唯一的参数

```js
postion = {
    // 当前位置的Coordinates 对象
    coords: {
    },
    // 时间戳DOMTimeStamp，表示获取到的位置的时间
    timestamp: 
}
```
- **error**：（可选）获取位置信息失败时的回调函数，使用 `PositionError` 对象作为唯一的参数
- **options**: （可选）一个可选的[PositionOptions](https://developer.mozilla.org/zh-TW/docs/Web/API/PositionOptions)对象

```js
options = {
    enableHighAccuracy: true, // 是否获取高精度定位，默认为false
    timeout: Infinity, // 最长可接受的定位返回时间，单位毫秒，默认为Infinity
    maximumAge: 0 // 等待请求的时间，单位毫秒，默认为0
}
```

#### [watchPosition()](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation/watchPosition)
 用于注册监听器，在设备的地理位置发生改变的时候自动被调用。也可以选择特定的错误处理函数。（该方法会返回一个 ID，如要取消监听可以通过 `Geolocation.clearWatch()` 传入该 ID 实现取消的目的）

**语法**：

```js
navigator.geolocation.watchPosition(success [, error] [, options])
```

```js
var watchID = navigator.geolocation.watchPosition(function(position) {
  do_something(position.coords.latitude, position.coords.longitude);
});

navigator.geolocation.clearWatch(watchID);
```