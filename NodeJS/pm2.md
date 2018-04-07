## 简介
PM2是node进程管理工具，主要功能有：性能监控、自动重启、负载均衡等

## 安装
```bash
npm i -g pm2
```

## 目录介绍
pm2安装好后，会自动创建下面目录
- `$HOME/.pm2`
- `$HOME/.pm2/logs`
- `$HOME/.pm2/pids`
- `$HOME/.pm2/pm2.log`
- `$HOME/.pm2/pm2.pid`
- `$HOME/.pm2/rpc.sock`
- `$HOME/.pm2/pub.sock`
- `$HOME/.pm2/conf.js`

## 入门教程
```bash
pm2 start ./bin/www --watch
```

## 常用命令
### 启动

```bash
pm2 start app.js
pm2 start app.js --name myapp # myapp为pm2进程名称
pm2 start app.js -i 0 # 根据cpu核数启动进程个数
pm2 start app.js --watch # 实时监控app.js的启动方式，当app.js文件有变动时，pm2会自动reload
```

**常用参数**：
- `--watch`：监听应用目录的变化，一旦发生变化，自动重启。如果要精确监听、不见听的目录，最好通过配置文件。
- `-i --instances`：启用多少个实例，可用于负载均衡。如果`-i 0`或者`-i max`，则根据当前机器核数确定实例数目。
- `--ignore-watch`：排除监听的目录/文件，可以是特定的文件名，也可以是正则。比如`--ignore-watch="test node_modules 'some scripts'"`
- `-n / --name`：应用的名称。查看应用信息的时候可以用到。
- `-o / --output <path>`：标准输出日志文件的路径。
- `-e / --error <path>`：错误输出日志文件的路径。
- `--interpreter <interpreter>`：the interpreter pm2 should use for executing app (bash, python...)。比如你用的coffee script来编写应用

### 查看进程
```bash
# pm2 show <app_id>
# pm2 info <app_id>
pm2 describe <app_id>
```

### 查看进程状态
```bash
pm2 list
```

### 监控
```bash
pm2 monit
```

### 停止
```bash
# 获取应用的名字（--name指定的）或者进程id
pm2 list

pm2 stop all # 停止pm2列表中所有的进程
pm2 stop <app_id|app_name> # 停止指定进程
```

### 重载
```bash
pm2 reload all # 重载pm2列表中所有的进程
pm2 reload <app_id|app_name> # 重载指定进程
```

### 重启
```bash
pm2 restart all # 重启pm2列表中所有的进程
pm2 restart <app_id|app_name> # 重启指定进程
```

### 删除进程
```bash
pm2 delete all # 删除所有pm2进程
pm2 delete <app_id|app_name> # 删除指定进程
```

### 日志操作
```bash
pm2 logs [--raw]   # Display all processes logs in streaming
pm2 flush          # Empty all log file
pm2 reloadLogs     # Reload all logs
```

## 配置文件
**简单说明**：
- 配置项和命令行参数基本是一一对应的
- 可以是`yaml`或`json`文件格式
- `json`格式的配置文件，pm2当作普通的js文件来处理，所以添加注释或编写代码
- 如果启动的时候指定了配置文件，则命令行参数会被忽略

```js
/* ecosystem.json */

{
  "apps" : [{
    // Application #1
    "name"        : "worker-app", // 应用名称
    "script"      : "worker.js", // 实际启动脚本
    "args"        : ["--toto=heya coco", "-d", "1"],
    "watch"       : true, // 是否开启监控模式，默认为false，也可以设置为要监控的目录/文件数组
    "node_args"   : "--harmony",
    "merge_logs"  : true,
    "cwd"         : "/this/is/a/path/to/start/script", // 当前工作路径
    "env": {
      "NODE_ENV": "development",
      "AWESOME_SERVICE_API_TOKEN": "xxx"
    },
    "env_production" : {
       "NODE_ENV": "production" // 环境参数，当前指定为生产环境
    },
    "env_staging" : {
       "NODE_ENV" : "staging",
       "TEST"     : true
    },
    "ignore_watch": [ // 从监控目录中排除
      "node_modules",
      "logs",
      "public"
    ],
    "exec_interpreter": "bash", // 应用程的脚本类型，默认是nodejs
    "min_uptime": "60s", // 最小运行事件，即应用程序在指定时间内退出，则认为程序异常退出，触发重启
    "max_restarts": 30, // 应用程序退出重启的次数，默认15次（从0开始计数）
    "exex_mode": "cluster_mode" // 应用程序启动模式，默认为fork，cluster_mode表示集群
  },{
    // Application #2
    "name"       : "api-app",
    "script"     : "api.js",
    "instances"  : 4,
    "exec_mode"  : "cluster_mode",
    "error_file" : "./examples/child-err.log", // 错误日志路径
    "out_file"   : "./examples/child-out.log", // 普通日志路径
    "pid_file"   : "./examples/child.pid" // 应用程序的pid文件路径
  }]
}
```

## 环境切换
在配置文件中通过`env_xx`来声明不同环境的配置，在启动应用时，通过`--env`参数指定运行的环境

```js
{
  // ...
  "env": { // pm2 start app.js
    "NODE_ENV": "production",
    "REMOTE_ADDR": "http://www.example.com/"
  },
  "env_dev": { // pm2 start app.js --env dev
    "NODE_ENV": "development",
    "REMOTE_ADDR": "http://wdev.example.com/"
  },
  "env_test": { // pm2 start app.js --env test
    "NODE_ENV": "test",
    "REMOTE_ADDR": "http://wtest.example.com/"
  }
}
```

## 负载均衡
```bash
pm2 start app.js -i 3   # 开启三个进程
pm2 start app.js -i 0   # 根据机器当前核数自动开启尽可能多的进程
pm2 start app.js -i max # 根据机器CPU核数，开启对应数目的进程
```

## 指令tab补全
```bash
pm2 completion install
source ~/.bash_profile
```

## 开机自启
**步骤**：
1. 通过`pm2 save`保存当前进程状态
2. 通过`pm2 startup [platform]`生成开机自启动命令
3. 将步骤2生成的命令，粘贴到控制台运行

## 传入node args
可通过命令或配置文件

**命令行**：
```bash
pm2 start app.js --node-args="--harmony"
```

**配置文件**：
```js
{
  "name" : "oc-server",
  "script" : "app.js",
  "node_args" : "--harmony"
}
```

## 远程部署

## 内存使用超过上限自动重启
```bash
pm2 start big-array.js --max-memory-restart 20M
```

## pm2 + nginx




- - - 
**参考**：
- [PM2实用入门指南](https://www.cnblogs.com/chyingp/p/pm2-documentation.html)