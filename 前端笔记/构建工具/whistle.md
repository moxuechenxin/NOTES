## 安装
```shell
npm install -g whistle
```

## 启动whistle
最新版本的whistle支持三种等价的命令`whistle`、`w2`、`wproxy`

**启动**
```shell
w2 start
```

**登陆启动**
```shell
w2 start -n <username> -w <psd>
```

**调试模式启动**  
(主要用于查看whistle的异常及插件开发)
```shell
w2 run 
```

**重启**
```shell
w2 start
```

**停止**
```shell
w2 stop
```
# 配置代理
## Rules
### 设置hosts
1. **指定www.ifeng.com的ip**
```
www.ifeng.com 127.0.0.1
# or
127.0.0.1 www.ifeng.com
```
2. **指定www.ifeng.com的ip和端口**  
把请求转发到本地8080端口，这个在平时开发中可以用来去掉url中的端口号
```
www.ifeng.com 127.0.0.1:8080
# or
127.0.0.1:8080 www.ifeng.com
```
3. **用某个域名的ip设置hosts**
```
www.ifeng.com host://www.qq.com:8080
# or
host://www.qq.com:8080 www.ifeng.com
```

### 本地替换
```
# Mac、Linux
www.ifeng.com file:///User/username/test
# or www.ifeng.com file:///User/username/test/index.html

# Windows的路径分隔符可以用 \ 或者 /
www.ifeng.com file://E:\xx\test
# or www.ifeng.com file://E:\xx\test\index.html
```

