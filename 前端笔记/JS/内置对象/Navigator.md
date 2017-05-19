## 说明
`navigator` 对象包含有关浏览器的信息

## 属性
属性 | 描述
---|---
cookieEnabled | 返回指明浏览器中是否启用 cookie 的布尔值
geolocation | 返回`geolocation`对象（IE9+）
platform | 返回运行浏览器的操作系统平台
appCodeName | 返回浏览器的代码名
appMinorVersion	| 返回浏览器的次级版本
appName	| 返回浏览器的名称
appVersion | 返回浏览器的平台和版本信息
browserLanguage	| 返回当前浏览器的语言
cpuClass | 返回浏览器系统的 CPU 等级
onLine | 返回指明系统是否处于脱机模式的布尔值
systemLanguage | 返回 OS 使用的默认语言
userAgent | 返回由客户机发送服务器的 user-agent 头部的值
userLanguage | 返回 OS 的自然语言设置

## [geolocation对象](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation/Using_geolocation)

### 属性


### 方法

#### [getCurrentPosition](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation/getCurrentPosition)
异步地请求获取用户位置，并查询定位硬件来获取最新信息。当定位被确定后，定义的回调函数就会被执行

**语法**：

```js
navigator.geolocation.getCurrentPosition(success [, error] [, options])
```

**参数**：
- **success**：成功得到位置信息时的回调函数，使用`Position`对象作为唯一的参数
- **error**：（可选）获取位置信息失败时的回调函数，使用 `PositionError` 对象作为唯一的参数
- **options**: （可选）一个可选的[PositionOptions](https://developer.mozilla.org/zh-TW/docs/Web/API/PositionOptions)对象

```js
options = {
    enableHighAccuracy: true, // 是否获取高精度定位，默认为false
    timeout: Infinity, // 最长可接受的定位返回时间，单位毫秒，默认为Infinity
    maximumAge: 0 // 等待请求的时间，单位毫秒，默认为0
}
```