## 说明
SSH是一种网络协议，用于计算机之间的加密登录
## 基本用法
```bash
ssh -p 3000 <user>@<host>
```
- `-p 3000`：表示登录的端口号，SSH的默认端口是22
- `user`：表示登录的用户名（如果本地用户名与远程用户名一致，登录时可以省略用户名）
- `host`：表示要登录的主机

## 公钥加密
### 流程
1. 远程主机收到用户的登录请求，把自己的公钥发给用户
2. 用户使用这个公钥，将登录密码加密后，发送回去
3. 远程主机用自己的私钥，解密登录密码，如果密码正确，就同意用户登录

### 中间人攻击 
攻击者插在用户与远程主机之间（比如在公共的wifi区域），用伪造的公钥，获取用户的登录密码。再用这个密码登录远程主机，那么SSH的安全机制就荡然无存了
## 公钥登录
### 流程
1. 将自己的公钥储存在远程主机上
2. 登录的时候，远程主机会向用户发送一段随机字符串，用户用自己的私钥加密后，再发回去
3. 远程主机用事先储存的公钥进行解密，如果成功，就证明用户是可信的，直接允许登录shell，不再要求密码

### 生成公钥
```bash
ssh-keygen -t rsa
```
指定rsa算法生成密钥，生成两个文件`id_rsa`(私钥)和`id_rsa.pub`(公钥)

Linux/Mac系统在 `~/.ssh`下，win系统在`/c/Documents and Settings/<username>/.ssh` 下

**将公钥传送到远程主机host上面**
```bash
ssh-copy-id <user>@<host>
```
从此你再登录，就不需要输入密码了

如果还是不行，就打开远程主机的/etc/ssh/sshd_config这个文件，检查下面几行前面"#"注释是否取掉
```bash
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```
然后，重启远程主机的ssh服务
```bash
# ubuntu系统
service ssh restart

# debian系统
/etc/init.d/ssh restart
```
## authorized_keys文件
远程主机将用户的公钥，保存在登录后的用户主目录的`$HOME/.ssh/authorized_keys`文件中。
公钥就是一段字符串，只要把它追加在authorized_keys文件的末尾就行了。
```bash
ssh <user>@<host> 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
```
单引号中间的部分，表示在远程主机上执行的操作；后面的输入重定向，表示数据通过SSH传向远程主机

## 远程操作
```bash
# 将$HOME/src/目录下面的所有文件，复制到远程主机的$HOME/src/目录
cd && tar czv src | ssh <user>@<host> 'tar xz'
```
```bash
# 将远程主机$HOME/src/目录下面的所有文件，复制到用户的当前目录
ssh <user>@<host> 'tar cz src' | tar xzv
```
```bash
# 查看远程主机是否运行进程httpd
ssh <user>@<host> 'ps ax | grep [h]ttpd'
```

## 绑定本地端口
```bash
# 让8080端口的数据，都通过SSH传向远程主机
ssh -D 8080 <user>@<host>
```
SSH会建立一个socket，去监听本地的8080端口。一旦有数据传向那个端口，就自动把它转移到SSH连接上面，发往远程主机

## 本地端口转发
**场景**：  
host1是本地主机，host2是远程主机，这两台主机之间无法连通。但是，另外还有一台host3，可以同时连通前面两台主机

**解决办法**：  
通过host3，将host1连上host2

> 在host1执行以下命令
```bash
ssh -L 2121:<host2>:21 <host3>

# 只要连接host1的2121端口，就等于连上了host2的21端口
ftp localhost:2121
```
指定SSH绑定本地端口2121，然后指定host3将所有的数据，转发到目标主机host2的21端口（假定host2运行FTP，默认端口为21)  
`L`参数一共接受三个值，分别是`本地端口:目标主机:目标主机端口`

```bash
# 将本机的5900端口绑定host3的5900端口（这里的localhost指的是host3，因为目标主机是相对host3而言的）
ssh -L 5900:localhost:5900 host3
```

```bash
# 通过host3的端口转发，ssh登录host2
ssh -L 9001:<host2>:22 host3

# 只要ssh登录本机的9001端口，就相当于登录host2了 (-p参数表示指定登录端口)
ssh -p 9001 localhost
```

## 远程端口转发
指绑定远程端口的转发  
**场景**：  
host1与host2之间无法连通，必须借助host3转发，host3是一台内网机器，它可以连接外网的host1，但是反过来就不行，外网的host1连不上内网的host3  

**解决办法**：  
从host3上建立与host1的SSH连接，然后在host1上使用这条连接就可以了

> 在host3执行下面的命令
```bash
# 让host1监听它自己的2121端口，然后将所有数据经由host3，转发到host2的21端口
ssh -R 2121:<host2>:21 <host1>
```
对于host3来说，host1是远程主机，所以这种情况就被称为"远程端口绑定"  
`R`参数也是接受三个值，分别是`远程主机端口:目标主机:目标主机端口`

> 在host1执行下面的命令
```bash
# 绑定之后，我们在host1就可以连接host2了
ftp localhost:2121
```
**注意**：  
"远程端口转发"的前提条件是，host1和host3两台主机都有sshD和ssh客户端
## SSH的其他参数
#### `N`参数，表示只连接远程主机，不打开远程shell；`T`参数，表示不为这个连接分配TTY。（这个两个参数可以放在一起用，代表这个SSH连接只用来传数据，不执行远程操作）
```bash
ssh -NT -D 8080 <host>
```

#### `f`参数，表示SSH连接成功后，转入后台运行。这样一来，你就可以在不中断SSH连接的情况下，在本地shell中执行其他操作
```bash
# 要关闭这个后台连接，就只有用kill命令去杀掉进程
ssh -f -D 8080 <host>
```

**参考**：
- [SSH原理与运用（一）：远程登录](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)
- [SSH原理与运用（二）：远程操作与端口转发](http://www.ruanyifeng.com/blog/2011/12/ssh_port_forwarding.html)
