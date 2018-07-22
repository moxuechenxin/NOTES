## 常用命令
```bash
# 检查配置是否正确
nginx -t 
# 检查指定配置文件是否正确
nginx -t -c <path/to/conf>
# 重载nginx
nginx -s reload
```

## Nginx简述
Nginx是一个开源且高性能、可靠的HTTP中间件、代理服务

### 特性
- **IO多路复用epoll**
  IO多路复用：多个描述符的I/O操作都能在一个线程内并发交替地顺序完成，‘复用’指地是复用同一个线程
- **轻量级**
  - 功能模块少
  - 代码模块化
- **CPU亲和(affinity)**
  是一种把CPU核心和Nginx工作进程绑定方式，把每个worker进程固定在一个cpu上进行，减少切换CPU地cache miss，获得更好地性能
- **sendfile**

## 安装
### CentOS 7.X
1. 添加文件yum源文件`/etc/yum.repos.d/nginx.repo`
```bash
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/<OS>/<OSRELEASE>/$basearch/
gpgcheck=0
enabled=1
```

2. `yum install nginx`

### 安装目录讲解
#### 查看安装目录
```bash
rpm -ql nginx
```

#### 目录讲解
路径 | 类型 | 作用
---|---|---
`/etc/logratate.d/nginx` | 配置文件 | Nginx日志轮转，用于logrotate服务的日志切割
`/etc/nginx/` | 目录、配置文件 | Nginx配置文件
`/etc/nginx/nginx.conf` |  | 
`/etc/nginx/conf.d/` | | 
`/etc/nginx/conf.d/default.conf` | | 
`/etc/nginx/fastcgi_params` |  配置文件 | cgi配置相关、fastcgi配置
`/etc/nginx/uwsgi_params` | | 
`/etc/nginx/scgi_params` | | 
`/etc/nginx/koi-utf` | 配置文件 | 编码转换映射转化文件
`/etc/nginx/koi-win` |  | 
`/etc/nginx/win-utf` |  | 
`/etc/nginx/mime.types` | 配置文件 | 设置http协议的Content-Type与扩展名对应关系
`/usr/lib/systemd/system/nginx-debug.service` | 配置文件 | 用于配置出系统守护进程管理器管理方式
`/usr/lib/systemd/system/nginx.service` | | 
`/etc/sysconfig/nginx` | | 
`/etc/sysconfig/nginx-debug` | | 
`/usr/lib64/nginx/modules/` | 目录 | Nginx模块目录
`/etc/nginx/modules` | | 
`/usr/sbin/nginx` | 命令 | Nginx服务的启动管理的终端命令
`/usr/sbin/nginx-debug` | | 
`/usr/share/doc/nginx-<version>` | 文件、目录 | Nginx的手册和帮助文件
`/usr/share/doc/nginx-<version>/COPYRIGHT` | | 
`/usr/share/man/man8/nginx.8.gz` | | 
`/var/cache/nginx` | 目录 | Nginx地缓存目录
`/var/log/nginx` | 目录 | Nginx的日志目录

#### 安装编译参数
```bash
# 查看可选编译参数
nginx -V
```

编译选项 | 作用
---|---
`--prefix=/etc/nginx` | 安装的目录和路径
`--sbin-path=/usr/sbin/nginx` | 
`--modules-path=/usr/lib64/nginx/modules` | 
`--conf-path=/etc/nginx/nginx.conf` | 
`--error-log-path=/var/log/nginx/error.log` | 
`--http-log-path=/var/log/nginx/access.log` | 
`--pid-path=/var/run/nginx.pid` | 
`--lock-path=/var/run/nginx.lock` | 
`--http-client-body-temp-path=/var/cache/nginx/client_temp` | 执行对应模块时，Nginx所保留的临时性文件
`--http-proxy-temp-path=/var/cache/nginx/proxy_temp` | 
`--http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp` | 
`--http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp` | 
`--http-scgi-temp-path=/var/cache/nginx/scgi_temp` | 
`--user=nginx` | 设定Nginx进程启动的用户和用户组
`--group=nginx` | 
`--with-cc-opt=parameters` | 设置额外的参数将被添加到CFLAGS变量
`--with-ld-opt=parameters` | 设置附加的参数，链接系统库

## 配置语法

### Nginx默认配置语法
```bash
# nginx.conf

user  nginx; # 设置nginx服务的系统使用用户
worker_processes  1; # 工作进程数

error_log  /var/log/nginx/error.log warn; # nginx的错误日志
pid        /var/run/nginx.pid; # nginx服务启动时候pid


events {
    worker_connections  1024; # 每个进程允许最大连接数
    # use  # 工作进程数
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

```bash
# server配置

server {
  listen 80; # 服务端口
  server_name localhost; # 服务访问地址(域名)

  location / {
    root /usr/share/nginx/html;
    index index.html index.htm;
  }

  error_page  404  /404.html;

  error_page   500 502 503 504  /50x.html;
  location = /50x.html {
      root   /usr/share/nginx/html;
  }
}

```

### HTTP请求
request：包括请求行、请求头部、请求数据  
response：包括状态行、消息报头、响应正文

### Nginx日志类型
包括：`error.log` `access.log`

**log_format**：
Syntax: log_format <name> [escape=default|json] string ...;      
Default: log_format combined "...";  
Context: http

#### Nginx 变量
- **HTTP请求变量**
  `arg_<PARAMETER>`、`http_<HEADER>`、`sent_http_<HEADER>`
  注意：全部为小写，且连接符改为`_`，譬如`http_user_agent`

- **内置变量**：Nginx内置的
- **自定义变量**：自己定义的

## Nginx模块
- Nginx官方模块
- 第三方模块

### 默认模块

**安装编译参数讲解**

编译选项 | 作用
---|---
`--with-http_stub_status_module` | Nginx的客户端状态 

**http_stub_status_module配置**：
```
Syntax: stub_status;
Default: --
Context: server,location
```
```bash
# *.conf开始

location /mystatus {
	stub_status;
}

# *.conf结束

# 输出如下信息
Active connections: 1 # 当前活跃的连接数
server accepts handled requests # nginx处理的握手的总次数，nginx处理的连接数，总的请求数
 9 9 9
Reading: 0 Writing: 1 Waiting: 0 # 正在读的数量，正在写的数量，等待的数量
```

编译选项 | 作用
---|---
`--with-http_random_index_module` | 目录中选择一个随机主页

**http_random_index_module配置**：
```bash
Syntax: random_index on | off;
Default: random_index off;
Context: location
```

```bash
# *.conf

location / {
  root   /usr/share/nginx/html;
  random_index on;
}
```

编译选项 | 作用
---|---
`--with-http_sub_module` | HTTP内容替换

**http_sub_module配置**：
```bash
Syntax: sub_filter string replacement;
Default: --;
Context: http, server, location
```

```bash
Syntax: sub_filter_last_modified on | off;
Default: sub_filter_last_modified off;
Context: http, server, location
```

```bash
Syntax: sub_filter_once on | off;
Default: sub_filter_once on;
Context: http, server, location
```

```bash
# *.conf

location / {
  root   /usr/share/nginx/html;
  index index.html index.htm;
  sub_filter '<a>' '<span>'; # 将<a>替换为<span>(默认只替换第一个<a>)
  sub_filter_once off; # 设置为全局替换
}
```

#### 请求限制
