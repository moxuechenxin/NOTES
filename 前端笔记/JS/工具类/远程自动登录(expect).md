## 工具
expect

**安装**:

> centos:
```bash
sudo yum install expect
```
> homebrew
```bash
brew install expect
```

## 四个命令
- `spawn`：启动新的进程
- `expect`：从进程接收字符串
- `send`：用于向进程发送字符串
- `interact`：允许用户交互

### spawn
`spawn`是启动新的进程

### expect
`expect`通常是用来等待一个进程的反馈。`expect`可以接收一个字符串参数，也可以接收正则表达式参数

注：`$expect_out(buffer)`存储了所有对expect的输入，`$expect_out(0, string)`存储了匹配到expect参数的输入

```bash
expect "hi\n"
send "you typed: $expect_out(buffer)"
send "but I only expected: $expect_out(0,string)"
```
当在标准输入中输入
```
test
hi
```

运算结果如下：
```bash
you typed: test
hi
I only expect: hi
```

单一分支模式语法：
```bash
expect "hi" {
  send "You said hi"
}
```

多分支模式语法：

```bash
expect "hi" { send "You said hi\n" } \
"hello" { send "Hello yourself\n" } \
"bye" { send "That was unexpected\n" }
```
等同于

```bash
expect {
  "hi" { send "You said hi\n"}
  "hello" { send "Hello yourself\n"}
  "bye" { send "That was unexpected\n"}
}
```

### send
`send`命令接收一个字符串参数，并将该参数发送到进程

### interact
> 在自动登录ftp后，允许用户交互

```bash
spawn ftp ftp.test.com
expect "Name"
send "user\r"
expect "Password:"
send "123456\r"
interact
```

### 实例

> 从机器A登录到机器B，然后执行机器B上的pwd命令，并停留在B机器上，等待用户交互

`#!/usr/bin/expect`告诉操作系统使用expect来执行

```bash
#!/usr/bin/expect
spawn ssh $BUser@$BHost
expect  "*password:" { send "$password\r" }
expect  "$*" { send "pwd\r" }
interact
```

> 
```bash
#!/usr/bin/expect -f 
set ip [lindex $argv 0 ]  //接收第一个参数,并设置IP 
set password [lindex $argv 1 ] //接收第二个参数,并设置密码 
set timeout 10     //设置超时时间 
spawn ssh root@$ip  //发送ssh请求
expect {     //返回信息匹配 
 "*yes/no" { send "yes\r"; exp_continue} //第一次ssh连接会提示yes/no,继续 
 "*password:" { send "$password\r" }  //出现密码提示,发送密码 
} 
interact   //交互模式,用户会停留在远程服务器上面
```

**参考**：
- [shell脚本实现ssh自动登录远程服务器示例](https://www.cnblogs.com/lqyye/p/7224268.html)