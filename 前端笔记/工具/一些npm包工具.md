### 本地服务器
- **[anywhere](https://www.npmjs.com/package/anywhere)**  
开启本地服务器，监听1234端口

```shell
anywhere -p1234
```

### 浏览器刷新
- **[browser-sync](http://www.browsersync.cn/docs/command-line/)**  

**静态网站**

```shell
browser-sync start --server --port 1234 --files "**" 
```
`--files` 路径是相对于运行该命令的项目（目录）; `"**"`表示监听当前目录下的所有文件  
（如果您需要监听多个类型的文件，您只需要用逗号隔开）

`--port` 监听的端口号

**动态网站**

如果您已经有其他本地服务器环境PHP或类似的，您需要使用代理模式。 BrowserSync将通过代理URL(localhost:3000)来查看您的网站

```shell
browser-sync start --proxy "Browsersync.cn" "css/*.css"
```


