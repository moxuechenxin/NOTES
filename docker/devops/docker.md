### docker-machine
```bash
docker-machine create <vm-name> # 创建一台安装了docker的linux虚拟机
docker-machine env <vm-name> # 输出指定虚拟机的docker环境变量
docker-machine stop <vm-name> # 停到指定docker虚拟机
eval $(docker-machine env <vm-name>) #将指定docker虚拟机的环境变量注入到本地系统环境

# 恢复本地docker server
docker-machine env --unset
eval $(docker-machine encv --unset)

# ssh登陆虚拟机
docker-machine ssh <vm-name>
```

### docker playground

### docker的架构和底层技术

### docker CI

命令	| 作用
---|---
image ls | 列出本地安装的镜像

```bash
# 构建镜像
docker build -t <image-name> <path/to/Dockerfile>
# 运行容器
docker run <container-name>

docker history <image-name>

# 输出所有container的id
docker container ls -aq
# 删除所有container
docker container rm ${docker container ls -aq}
```

### Dockerfile
> https://blog.csdn.net/mozf881/article/details/55798811

```bash
FROM scratch  #指定使用哪个镜像源(scratch表示没有依赖)

LABEL maintainer="zhengji1991@gmail.com"
LABEL version="1.0"
LABEL description="This is description"

# 设置常量
ENV MYSQL_VERSION 5.6

# 执行命令并创建新的Image Layer(为了美观，复杂的RUN尽量用反斜线换行，避免无用分层，合并多条命令成一行)
RUN yum install -y vim

# 设置容器启动后默认执行的命令和参数
# 如果docker run指定了其他命令，CMD命令会被忽略
# 如果定义了多个CMD，只有最后一个会执行
CMD echo "hello $name"

# 设置容器启动时运行的命令
# 让容器以应用程序或者服务的形式运行
# 不会被忽略，一定会执行
# 最佳实践：执行一个shell脚本
# COPY docker-entrypoint.sh /usr/local/bin
ENTRYPOINT ["docker-entrypoint.sh"]

# 设定当前工作目录（类似于cd命令）
WORKDIR /root # 如果没这个目录，则会创建

# 大部分情况，COPY优于ADD
# ADD除了COPY还有额外功能（解压）
# 添加远程文件／目录应使用curl或wget
ADD <path/to/file> <image/path>
COPY <path/to/file> <image/path>

VOLUME 

EXPOSE
``` 

### container
在image layer(只读)之上建立一个container layer(可读写)

image负责app的存储和分发，container负责运行app

```bash
# docker commit
docker container commit 

# docker build
docker image build
```
