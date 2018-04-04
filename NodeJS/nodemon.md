## 安装
可以全局安装，也可以安装在本地项目目录
```bash
npm i -g nodemon

# 或者

npm install --save-dev nodemon
```

## 用法
用nodemon来代替node启动应用
```bash
nodemon [your node app]
```

如果没有在应用中指定端口，可以在命令中指定
```bash
nodemon ./server.js localhost 8080
```

debug模式
```bash
nodemon --inspect ./server.js 80
```

## 配置文件
nodemon.json
```json
{
  "restartable": "rs",
  "ignore": [
    ".git",
    "node_modules/**/node_modules"
  ],
  "verbose": true,
  "execMap": {
    "js": "node --harmony"
  },
  "events": {
    "restart": "osascript -e 'display notification \"App restarted due to:\n'$FILENAME'\" with title \"nodemon\"'"
  },
  "watch": [
    "test/fixtures/",
    "test/samples/"
  ],
  "env": {
    "NODE_ENV": "development",
    "PORT": "3000"
  },
  "ext": "js json"
}
```

**参数说明**:
- **`restartable`**：默认`rs`，默认是`rs` ，可以是任意字符串。当用 `nodemon` 启动应用时，可以直接键入 `rs` 直接重启服务。除了字符串值外，还可以设置 `false` 值，这个值的意思是当 `nodemon` 影响了你自己的终端命令时，设置为 `false` 则不会在 `nodemon` 运行期间监听 `rs` 的重启命令

- **`ignore`**：忽略的文件或者文件夹路径(相对于 nodemon.json 所在位置的路径)

- **`ignoreRoot`**：默认忽略参数，nodemon默认忽略的目录列表，默认值：`[".git", "node_modules", "bower_components", ".sass-cache"]`

- **`verbose`**：为`true`时表示输出详细启动与重启信息

- **`execMap`**：运行服务的后缀名和对应的运行命令（`"js"`: `"node --harmony"` 表示用 `nodemon` 代替 `node  --harmony` 运行 js 后缀文件；`""` 指 `www` 这些没有后缀名的文件；默认的 `defaults.js` 配置文件会识别一些文件：`py: 'python',rb: 'ruby'`）

- **`events`**：表示nodemon运行到某些状态时的一些触发事件，总共有五个状态：
  - `start`：子进程（即监控的应用）启动
  - `crash`：子进程崩溃，不会触发 exit
  - `exit`：子进程完全退出，不是非正常的崩溃
  - `restart`：子进程重启
  - `config:update `：nodemon 的 config 文件改变

- **`watch`**：监控的文件夹路径或者文件路径

- **`env`**：运行环境`development`是开发环境，`production` 是生产环境。`port` 是端口号

- **`ext`**：监控指定后缀名的文件，用空格间隔。默认监控的后缀文件：`js json coffee litcoffee`。

**注**：关于监控以及忽略文件修改有个顺序的问题，或者说优先级，首先 nodemon 会先读取 watch 里面需要监控的文件或文件路径，再从文件中选择监控`ext`中指定的后缀名，最后去掉从`ignore`中指定的忽略文件或文件路径

- **`legacy-watch`**：nodemon 使用  Chokidar 作为底层监控系统，但是如果监控失效，或者提示没有需要监控的文件时，就需要使用轮询模式（polling mode），即设置 legacy-watch 为 true，也可以在命令行中指定`nodemon --legacy-watch`或者`nodemon -L`

- **`colours`**：默认true，输出信息颜色标示

- **`runOnChangeOnly`**：默认为false，一开始就启动应用并监控文件改动（为`true`时运行 nodemon www 项目不会启动，只保持对文件的监控，当监控的文件有修改并保存时才会启动应用）

- **`stdin，stdout`**：默认均为true，关于标准输入输出的设置

- - - 
**参考**：
- [nodemon 基本配置与使用](https://www.cnblogs.com/JuFoFu/p/5140302.html)