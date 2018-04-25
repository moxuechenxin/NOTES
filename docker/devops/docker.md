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

### CentOS安装docker
> [参考](https://docs.docker.com/install/linux/docker-ce/centos/)

```bash
# 第一步：移除旧版本
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine

# 第二步： 安装
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2

sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install docker-ce

# 第三步： 启动
sudo systemctl start docker
```

### docker playground

### docker的架构和底层技术

### docker CI

命令	| 作用
---|---
image ls | 列出本地安装的镜像

```bash
docker history <image-name>

docker ps -a # 列出所有的容器信息，包括未运行的

docker pull [OPTIONS] NAME[:TAG|@DIGEST] # 从镜像仓库中拉取或者更新指定镜像

# 在运行的容器中交互式地执行命令
docker exec -it <container-id> <COMMAND> <ARG>...
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

```bash
# 构建镜像
docker build -t <image-name> <path/to/Dockerfile>
# 运行容器
docker run <image-name> # 如果指定镜像不存在，则先下载镜像再运行
```

### container
在image layer(只读)之上建立一个container layer(可读写)

image负责app的存储和分发，container负责运行app

```bash
# docker commit
docker container commit 

# docker build
docker image build

# 运行容器
docker run <image-name>
# 交互式运行容器
docker run -it <image-name>
# 输出所有container的id
docker container ls -aq
# 删除所有container
docker container rm ${docker container ls -aq}
```

### docker NETWORK
```bash
ip a
ip link

# 查看本机网络命名空间列表
ip netns list

# 删除命名空间
ip netns delete <name-space>

# 添加命名空间
ip netns add <name-space>

# 在指定命名空间内执行命令
ip netns exec <name-space> <command> <arg1> ...
ip netns exec <name-space> ip link
ip netns exec <name-space> ip link set dev <link-name> up

ip netns exec <name-space> ip addr add 192.168.1.1/24 dev <link-name>
```
```bash
# 查看docker的网络列表
docker network ls

docker network inspect <network-id>
```

容器之间的link
```bash
docker run --name <container-name> --link <link-to-container-name> <image-name> 
```
```bash
# 创建一个driver为bridge的network
docker network create -d bridge <network-name>
# 指定生成容器的network
docker run <image-name> --network <network-name>
# 将指定container连接到指定network
# 连接到用户自定义driver为bridge的network的container之间默认已互相连接
docker network connect <network-name> <container-name>
```

容器的端口映射
```bash
docker run -p <宿主端口>:<容器内部端口> <image-name>
```

overlay和underlay  
不同主机的docker之间怎么通信
```
```

### docker的持久化储存和数据共享
**方案**：
1. 基于本地文件系统的Volume
2. 基于plugin的Volume

**Volume的类型**：
1. 受管理的Data Volume，由docker后台自动创建
2. 绑定挂载的Volume(Bind Mounting)，具体挂载位置可以由用户指定

```bash
# Data Volume
1. 在Dockerfile中指定VOLUME挂载路径
2. docker run -v <volume-name>:<容器的目录(在Dockerfile中指定VOLUME挂载路径)> <image-name>

docker volume ls
docker volume inspect <volume-name>
docker volume rm <volume-name>
```

```bash
# Bind Mounting
1. docker run -v <主机的目录>:<容器的目录(在Dockerfile中指定VOLUME挂载路径)> <image-name>
```

### docker-compose
- Docker Compose是一个工具(linux系统需要单独安装)
- 这个工具可以痛殴一个yml文件定义多容器的docker应用
- 通过一条命令就可以根据yml文件的定义去创建或者管理这几个容器

docker-compose.yml
#### 三大概念
1. Service
2. Networks
3. Volumes

##### Services
- 一个service代表一个container，这个container可以从dockerhub的image来创建，或者从本地的Dockerfile build出来的image来创建
- Service的启动类似docker run,我们可以给其指定network和volume，所以可以给service指定network和Volume的引用

```bash
version: '3'

services:

  wordpress:
    image: wordpress
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_PASSWORD: root
    networks:
      - my-bridge

  mysql:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: wordpress
    volumes:
      - mysql-data:/var/lib/mysql
    networks:
      - my-bridge

volumes:
  mysql-data:

networks:
  my-bridge:
    driver: bridge
```
```bash
version: "3"

services:

  redis:
    image: redis

  web:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 8080:5000
    environment:
      REDIS_HOST: redis
```

```bash
# 以当前目录的docker-compose.yml作为配置文件
docker-compose up
docker-compose -d up # 后台执行

docker-compose -f <path/to/compose.yml> up

docker-compose ps

docker-compose stop
docker-compose start

# 停止并删除containers，networks，images，volumes
docker-compose down

# 列出containers和对应的images
docker-compose images

docker exec <service-name> <command> <arg1>...

docker-compose build
```

### 容器编排swarm
搭建swarm cluster setup三种方法：
- Vagrant + Virtualbox
- Docker Machine + Virtualbox
- Play with docker

```bash
docker swarm init --advertise-addr <ip>

docker node ls # 显示swarm中的当前节点
```

#### service
```bash
docker service create # 类似于docker run
docker service create --name demo busybox sh -c "while true;do sleep 3600;done;"
docker service ls
docker service ps demo

docker service scale SERVICE=REPLICAS [SERVICE=REPLICAS...] [flags]
docker service scale demo=5

docker service rm demo

docker service create --name my-mysql --env MYSQL_ROOT_PASSWORD=root --env MYSQL_DATABASE=wordpress --mount type=volume,source=mysql-data,destination=/var/lib/mysql mysql

nslookup <site>
nslookup tasks.<service-name>
```

#### Routing Mesh
两种体现：
1. **Internal**：Container和Container之间的访问通过overlay网络（通过虚拟IP）
2. **Ingress**：如果服务有绑定接口，则此服务可以通过任意swarm节点的相应接口访问

**Ingress Network**：
- 外部访问的负载均衡
- 服务端口被暴露到各个swarm节点
- 内部通过IPVS进行负载均衡

#### stack
```
docker stack deploy <stack-name> --compose-file=docker-compose.yml
docker stack ls
docker stack ps <stack-name>
docker stack rm <stack-name>
docker stack services <stack-name>
```

#### DockerSecret
**Secret**:
- 用户名密码
- SSH Kwy
- TLS认证
- 任何不想让别人看到的数据

**Secret Management**:
- 存在Swarm Manager节点Raft database里
- Secret可以assign给一个service，这个service就能看到这个在container内部Secret
- 在container内部Secret看起来像文件，但是实际是在内存中

```bash
# 两种方式：文件、标准输入
docker secret create <secret-name> <path/to/file>
echo "mypassword" | docker secret create <secret-name> -
# 存放在容器内的/run/secrets/目录

docker secret ls
docker secret rm <secret-name>

# 使用secret
docker service create --name client --secret my-password busybox sh -c "while true; do sleep 3600; done"
```

#### service更新
**步骤**：
1. 创建service
2. scale service
3. update service：
```bash
# 更新image
docker service update --image <new-image-name> <service-name>
# 更新端口映射
docker service update --publish-rm 8080:5000 --publish-add 8088:5000 <service-name>
```