## 简介
Docker可以从`Dockerfile`中一步一步的读取指令来自动创建镜像，常使用`Dockerfile`来创建用户自定义镜像，docker是严格按照顺序来运行指令的

### FROM
指定基础镜像，如果指定镜像在本地不存在则会从公共仓库下载
```bash
# scratch表示没有基础镜像
FROM <image>:<tag>
```

## RUN、ENTRYPOINT、CMD
### 运行时机
**RUN**：在Dockerfile构建镜像的过程(Build)中运行，最终被commit到镜像中  
**ENTRYPOINT／CMD**：在容器运行run、start时运行

### shell形式和exec形式：
```bash
# shell形式
RUN/ENTRYPOINT/CMD <command> <param1> <param2> ...

# exec形式（注意使用双引号）
RUN/ENTRYPOINT/CMD ["<executable>", "<param1>", "<param2>", ...]
```
**差异**：  
1. shell形式提供了默认的指令`/bin/sh -c`，所以其指定的command将在shell的环境下运行，如果镜像中不包含`/bin/sh`，容器会无法启动

### 重载
1. Dockerfile中只有最后一个`CMD`指令会生效
2. Dockerfile中只有最后一个`ENTRYPOINT`指令会生效
3. `CMD`指定的命令会被`docker run`传递的命令覆盖
4. `ENTRYPOINT`指定的命令不会被`docker run`传递的命令覆盖，容器名后面的所有内容都当成参数传递给其指定的命令，并且会覆盖`CMD`所指定的参数（如果有的话）。可以在run时使用`--entrypoint`来覆盖`ENTRYPOINT`指令
5. 当`CMD`和`ENTRYPOINT`同时使用时，`CMD`因为被当作`ENTRYPOINT`(exec形式)的参数而无法运行。可以使用如下方法同时运行`CMD`和`ENTRYPOINT`：
> 把ENTRYPOINT需要运行的命令写到shell脚本中，并在该脚本中使用exec "$@"来运行CMD中的命令
```bash
# Dockerfile

FROM busybox
MAINTAINER binss(i@binss.me)

COPY ./entrypoint.sh /
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["echo","CMD"]
```
```bash
#!/bin/sh
set -e

echo "ENTRYPOINT"

exec "$@"
```

### EXPOSE
`EXPOSE`指令告诉容器在运行时要监听的端口，但是这个端口是用于多个容器之间通信用的（links），外面的host是访问不到的。要把端口暴露给外面的主机，在启动容器时使用`-p <宿主端口>:<容器端口>`选项
```bash
# expose memcached(s) port
EXPOSE 11211 11212
```

### ADD
```bash
ADD <src> <dest>
```
在build镜像的时候，将文件`<src>`拷贝到container的文件系统对应的路径`<dest>`下

`<src>`：可以是文件、文件夹、URL，对于文件和文件夹`<src>`必须是在Dockerfile的相对路径下（build context path），即只能是相对路径且不能包含`../path/`  
`<dest>`：只能是容器中的绝对路径。如果路径不存在则会自动级联创建，根据你的需要是<dest>里是否需要反斜杠/，习惯使用/结尾从而避免被当成文件
```bash
# 支持模糊匹配
ADD hom* /mydir/        # adds all files starting with "hom"
ADD hom?.txt /mydir/    # ? is replaced with any single character

ADD requirements.txt /tmp/
RUN pip install /tmp/requirements.txt
ADD . /tmp/
```
- `ADD`支持远程URL获取文件，但官方认为是`strongly discouraged`，建议使用`wget`或`curl`代替
- `ADD`还支持自动解压tar文件，比如`ADD trusty-core-amd64.tar.gz /`会线自动解压内容再`COPY`到在容器的`/`目录下

### COPY
`COPY`的语法与功能与`ADD`相同，只是不支持上面讲到的`<src>`是远程URL、自动解压这两个特性，建议尽量使用`COPY`，并使用`RUN`与`COPY`的组合来代替`ADD`，这是因为虽然`COPY`只支持本地文件拷贝到`container`，但它的处理比`ADD`更加透明，建议只在复制`tar`文件时使用`ADD`，如`ADD trusty-core-amd64.tar.gz /`

### ENV
用于设置环境变量：
```bash
ENV <key> <value>
```
设置了后，后续的`RUN`命令都可以使用，当运行生成的镜像时这些环境变量依然有效，如果需要在运行时更改这些环境变量可以在运行`docker run`时添加`-env <key>=<value>`参数来修改

### VOLUME
VOLUME指令用来在容器中设置一个挂载点，可以用来让其他容器挂载以实现数据共享或对容器数据的备份、恢复或迁移

### WORKDIR
WORKDIR指令用于设置Dockerfile中的RUN、CMD和ENTRYPOINT指令执行命令的工作目录(默认为/目录)，该指令在Dockerfile文件中可以出现多次，如果使用相对路径则为相对于WORKDIR上一次的值，例如`WORKDIR /a`，`WORKDIR b`，`RUN pwd`最终输出的当前目录是`/a/b`。（`RUN cd /a/b`，`RUN pwd`是得不到`/a/b`的）如果没这个目录，则会创建

### ONBUILD
`ONBUILD`指令用来设置一些触发的指令，用于在当该镜像被作为基础镜像来创建其他镜像时(也就是Dockerfile中的`FROM`为当前镜像时)执行一些操作，`ONBUILD`中定义的指令会在用于生成其他镜像的Dockerfile文件的`FROM`指令之后被执行，上述介绍的任何一个指令都可以用于`ONBUILD`指令，可以用来执行一些因为环境而变化的操作，使镜像更加通用

**注意**：
1. ONBUILD中定义的指令在当前镜像的build中不会被执行。
2. 可以通过查看docker inspect <image>命令执行结果的OnBuild键来查看某个镜像ONBUILD指令定义的内容。
3. ONBUILD中定义的指令会当做引用该镜像的Dockerfile文件的FROM指令的一部分来执行，执行顺序会按ONBUILD定义的先后顺序执行，如果ONBUILD中定义的任何一个指令运行失败，则会使FROM指令中断并导致整个build失败，当所有的ONBUILD中定义的指令成功完成后，会按正常顺序继续执行build。
4. ONBUILD中定义的指令不会继承到当前引用的镜像中，也就是当引用ONBUILD的镜像创建完成后将会清除所有引用的ONBUILD指令。
5. ONBUILD指令不允许嵌套，例如ONBUILD ONBUILD ADD . /data是不允许的。
6. ONBUILD指令不会执行其定义的FROM或MAINTAINER指令

### USER
为运行镜像时或者任何接下来的RUN指令指定运行用户名或UID
```bash
USER daemon
```

### LABEL
添加镜像元信息
```bash
LABEL maintainer="zhengji1991@gmail.com" # 镜像署名作者
LABEL version="1.0"
LABEL description="This is description"
```

### .dockerignore
.dockerignore用来忽略上下文目录中包含的一些image用不到的文件，它们不会传送到docker daemon。规则使用go语言的匹配语法
```bash
.git
tmp*
```

- - -
参考：
- [Dockerfile指令详解](http://seanlook.com/2014/11/17/dockerfile-introduction/)
- [Docker--从入门到实践](https://yeasy.gitbooks.io/docker_practice/content/image/dockerfile/cmd.html)
- [跟我一起学Docker——RUN、ENTRYPOINT与CMD](https://www.binss.me/blog/learn-docker-with-me-about-run-entrypoint-and-cmd/)