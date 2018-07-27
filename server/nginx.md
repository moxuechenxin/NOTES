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
- 连接频率限制：`limit_conn_module`  
- 请求频率限制：`limit_req_module`

HTTP协议版本 | 连接关系
---|---
HTTP1.0 | TCP不能复用
HTTP1.1 | 顺序性TCP复用
HTTP2.0 | 多路复用TCP复用

**HTTP协议的连接与请求**：
- HTTP请求建立在一次TCP连接基础上   
- 一次TCP请求至少产生一次HTTP请求

**连接限制**：
```bash
Syntax: limit_conn_zone key zone=name:size;
Default: --;
Context: http;
```
```bash
Syntax: limit_conn zone number;
Default: --;
Context: http, server, location;
```
```bash
http {
  # ....

  limit_conn_zone $binary_remote_addr zone=conn_zone:1m;

  server {
    # ....
    location / {
      root /usr/share/nginx/html;
      limit_conn conn_zone 1;
      index index.html index.htm;
    }
  }
}
```

**请求限制**：
```bash
Syntax: limit_req_zone key zone=name:size rate=rate;
Default: --;
Context: http;
```

```bash
Syntax: limit_req zone=name [burst=number] [nodelay];
Default: --;
Context: http, server, location;
```
```bash
http {
  # ....

  limit_req_zone $binary_remote_addr zone=req_zone:1m rate=1r/s;

  server {
    # ....
    location / {
      root /usr/share/nginx/html;
      limit_req zone=req_zone;
      index index.html index.htm;
    }
  }
}
```

#### 访问限制
- 基于IP的访问控制：`http_access_module`
- 基于用户的信任登陆：`http_auth_basic_module`

**http_access_module**：
```bash
Syntax: allow address | CIDR | unix: | all;
Default: --;
Context: http, server, location, limit_except;
```

```bash
# address => remote_addr
Syntax: deny address | CIDR | unix: | all;
Default: --;
Context: http, server, location, limit_except;
```

```bash
http {
  # ....

  server {
    # ...

    location ~  ^/api {
      allow 125.55.55.4/24;
      deny all;
    }
  }
}
```

**局限性**：
- 只能通过`$remote_addr`控制信任

**解决方案**：
1. 采用别的HTTP头信息控制访问，如：`HTTP_X_FORWARD_FOR`（可串改，不一定准确）
2. 结合geo模块
3. 通过HTTP自定义变量传递

**`http_x_forwarded_for`**：
![http_x_forwarded_for](/.assets/images/http_x_forwarded_for.png)

**http_auth_basic_module**：
```bash
Syntax: auth_basic string | off;
Default: auth_basic off;
Context: http, server, location, limit_except;
```
```bash
# yum install httpd-tools
# 可使用htpasswd生成密码文件
# htpasswd -c </path/to/file> <username>
Syntax: auth_basic_user_file file;
Default: --;
Context: http, server, location, limit_except;
```
```bash
http {
  # ...

  server {
    # ....

    location ~ ^/admin.html {
      # ...
      auth_basic "Auth access test! Input your password!";
      auth_basic_user_file <path/to/file>;  
    }
  }
}
```

**局限性**：
- 用户信息依赖文件方式
- 操作管理机械，效率地下

**解决方案**：
- Nginx结合LUA实现高效验证
- Nginx和LDAP打通，利用nginx-auth-ldap

## 常见Nginx中间件架构
- 静态资源WEB服务
- 代理服务
- 负载均衡调度器SLB
- 动态缓存

### 静态资源WEB服务
##### 静态资源服务场景-CDN

**文件读取**：
引读：`--with-file-aio`异步文件读取

```bash
Syntax: sendfile on | off;
Default: sendfile off;
Context: http, server, location, if in location;
```

**tcp_nopush**：
作用：sendfile开启的情况下，提高网络包的传输效率

```bash
Syntax: tcp_nopush on | off;
Default: tcp_nopush off;
Context: http, server, location;
```

**tcp_nodelay**：
作用：keepalive连接下，提高网络包的传输实时性

```bash
Syntax: tcp_nodelay on | off;
Default: tcp_nodelay off;
Context: http, server, location;
```

**压缩（gzip）**：
作用：压缩传输

```bash
Syntax: gzip on | off;
Default: gzip off;
Context: http, server, location, if in location;
```
```bash
Syntax: gzip_comp_level level;
Default: gzip_comp_level 1;
Context: http, server, location;
```
```bash
Syntax: gzip_http_version 1.0 | 1.1;
Default: gzip_http_version 1.1;
Context: http, server, location;
```
```bash
# Syntax: gzip_types mimes;
# Default: ;
# Context: http, server, location;
```

**扩展Nginx压缩模块**：
- `http_gzip_static_module`：预读gzip功能（文件已gzip压缩）
- `http_gunzip_module`：应用支持gunzip的压缩方式

```bash
server {
    # ...

    sendfile on;
    
    location ~ .*\.(jpg|gif|png)$ {
        gzip on;
        gzip_http_version 1.1;
        gzip_comp_level 2;
        gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
        root  /opt/app/code/images;
    }

    location ~ .*\.(txt|xml)$ {
        gzip on;
        gzip_http_version 1.1;
        gzip_comp_level 1;
        gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
        root  /opt/app/code/doc;
    }

    location ~ ^/download {
        # 访问/download/test.img 返回 /download/test.img.gz
        gzip_static on;
        tcp_nopush on;
        root /opt/app/code;
    }
}
```

#### 浏览器缓存
HTTP协议定义的缓存机制（如：Expires； Cache-Control等）

![http-cache](/.assets/images/http-cache.png)

**配置语法**：
添加Cache-Control、Expires头

```bash
Syntax: expires [modified] time;
        expires epoch | max | off;
Default: expires off;
Context: http, server, location, if in location;
```

```bash
server {
  # ....
  location ~ .*\.(css|js)$ {
    expires 365d;
  }
}
```

#### 跨域请求
```bash
Syntax: add_header name value [always];
Default: --;
Context: http, server, location, if in location;
```

```bash
server {
  # ...

  location ~ ^/api {
    add_header Access-Control-Allow-Origin http://www.a.com;
    add_header Access-Control-Allow-Methods GET, POST;
    # ...
  }
}
```

#### 防盗链
##### 基于http_referer防盗链
```bash
server {
  # ...

  location ~ .*\.(png|jpg|gif)$ {
    valid_referers none blocked www.mydomain.com mydomain.com;
 
    if ($invalid_referer) {
      return   403;
    }
  }
}
```

#### 代理服务
![nginx-proxy](/.assets/images/nginx-proxy.png)

**正向代理**：
正向代理的对象是客户端

**反向代理**：
正向代理的对象是服务端

**配置语法**：
```bash
Syntax: proxy_pass URL;
Default: --;
Context: location, if in location, limit_except;
```
```bash
# URL有以下几种类型：
http://localhost:8000/uri/
https://192.168.1.1:8000/uri/
http://unix:/tmp/backend.socket:/uri/
```

##### 缓冲区
```bash
# 扩展proxy_buffer_size、proxy_buffers、proxy_busy_buffers_size、proxy_max_temp_file_size
Syntax: proxy_buffering on | off;
Default: proxy_buffering on;
Context: http, server, location;
```

##### 跳转重定向
```bash
Syntax: proxy_redirect default; | proxy_redirect off; | proxy_redirect redirect replacement;
Default: proxy_redirect default;
Context: http, server, location;
```

##### 头信息
```bash
# 扩展：proxy_hide_header、proxy_set_body
Syntax: proxy_set_header field value;
Default: proxy_set_header Host $proxy_host; | proxy_set_header Connection close;
Context: http, server, location;
```

##### 超时
```bash
# 扩展：proxy_read_timeout、proxy_send_timeout
Syntax: proxy_connect_timeout time;
Default: proxy_connect_timeout 60s;
Context: http, server, location;
```

#### 负载均衡
**GSLB与SLB（Nginx）**：

![GSLB](/.assets/images/GSLB.png)
![SLB](/.assets/images/SLB.png)


**网络模型**：分为四层负载均衡和七层负载均衡（Nginx）

```bash
# 必须放在server层外
Syntax: upstream name {...};
Default: --;
Context: http;
```

```bash
http {
  # ...

  upstream imooc {
    server 127.0.0.1:8001;
    server 127.0.0.1:8002;
    server 127.0.0.1:8003;
  }

  server {
    listen 80;
    server_name localhost;

    location / {
      proxy_pass http://imooc;
    }
  }
}
```

##### upstream

```bash
upstream backend {
  server backend1.example.com weight=5; # weight表示该server的权重
  server backend1.example.com:8080;
  server unix:/tmp/backend3;

  server backup1.example.com:8080 backup; # backup表示备份节点
  server backup2.example.com:8080 backup;
}
```


> 后端服务器在负载均衡调度中的状态：
- `down`： 当前的server暂时不参与负载均衡
- `backup`： 预留的备份服务器
- `max_fails`： 允许请求失败的次数
- `fail_timeout`： 经过max_fails失败后，服务暂停的时间
- `max_conns`： 限制最大的接收的连接数

```bash
upstream backend {
  server backend1.example.com:8001 down;
  server backend1.example.com:8002 backup;
  server backend1.example.com:8003 max_fails=1 fail_timeout=10s;
}
```

> 调度算法
- `轮询`：按时间顺序逐一分配到不同的后端服务器
- `加权轮询`：weight值越大，分配到的访问几率越高
- `ip_hash`：每个请求按访问IP的hash结果分配，这样来自同一个IP的固定访问一个后端服务器
- `url_hash`：按照访问的URL的hash结果来分配请求，是每个URL定向到同一个后端服务器
- `least_conn`：最少链接数，哪个机器连接数少就分发
- `hash关键数值`：hash自定义的key

```bash
# ip_hash

upstream backend {
  ip_hash; # 基于$remote_addr
  server backend1.example.com:8001;
  server backend1.example.com:8002;
  server backend1.example.com:8003;
}
```
```bash
# url_hash (nginx版本 >= 1.7.2)
Syntax: hash key [consistent];
Default: --;
Context: upstream;
```
```bash
# url_hash

upstream backend {
  url_hash $request_uri;
  server backend1.example.com:8001;
  server backend1.example.com:8002;
  server backend1.example.com:8003;
}
```

#### Nginx缓存服务
**缓存类型**：
- 服务端缓存
- 代理缓存
- 客户端缓存

**代理缓存**：
![proxy-cache](/.assets/images/proxy-cache.png)

**proxy_cache配置语法**：
```bash
Syntax: proxy_cache_path path [levels=levels] [use_temp_path=on|off] keys_zone=name:size [inactive=time] [max_size=size] [manager_files=number] [manager_sleep=time] [manager_threshold=time] [loader_files=number] [loader_sleep=time] [loader_threshold=time] [purger=on|off] [purger_files=number] [purger_sleep=time] [purger_threshold=time];
Default: --;
Context: http;
```
```bash
Syntax: proxy_cache <proxy_cache_path> | off;
Default: proxy_cache off;
Context: http, server, location;
```
```bash
Syntax: proxy_cache_valid [code ...] time;
Default: --;
Context: http, server, location;
```
```bash
# 缓存的维度
Syntax: proxy_cache_key string;
Default: proxy_cache_key $scheme$proxy_host$request_uri;
Context: http, server, location;
```