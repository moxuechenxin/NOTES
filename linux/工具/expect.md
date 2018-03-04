## 概述
`expect`是用来实现自动和交互式任务进行通信，而无需人的干预

**命令行**：
```bash
expect [-dDinN] [-c cmds] [ [-[f|b]] cmdfile ] [args]
```
`cmdfile`脚本文件以`.exp`作为后缀（非必须），在文件第一行添加如下标识(mac)，告诉shell通过Expect解析本文件：
```bash
#!/usr/bin/expect
```

**选项**：

选项 | 说明
---|---
-c | 可执行命令的前置符，其后的命令应该被引起来，该选项可以使用多次，每个-c可以跟多个以分号分隔的命令。命令按照出现的顺序执行,例如：`expect -c "puts first\n; puts second" -c "puts three"`
-d | 输出一些诊断信息，命令执行时的内部动作。当你写的脚本和预期不符时，可用此项来调试脚本
-D | 交互式的调试器，类似gdb。适合专业人士使用
-f | 指定Expect读取的文件，如果文件是`-`，则表示是从标准输入读取。该选项会将文件一次性全部读入内存，该选项是可选的
-b | 类似-f选项，只是每次只读取一行
-i | 以交互的方式运行`expect`，等效直接敲`expect`
-- | 可以用来界定选项的结束，此项可以用在当你想传递一个类似选项的参数时，防止`Expect`误认为是选项
-N/-n | 如果$exp_library/expect.rc和~/.expect.rc存在，Expect会分别自动读取，若要阻止此过程则需要分别指定-N和-n。此项一般用不上。
-v | 输出版本号并退出

可选项`args`的内容以列表的形式被保存在`$argv`中，`$argc`被初始化为`argv`的长度（个数），`$argv0`保存脚本的名字

**Expect工作原理**：  
`expect`脚本由一系列`expect-send`对组成：`expect`等待输出中输出特定的字符，通常是一个提示符，然后发送特定的响应

## 命令
Expect使用Tcl的语法，又进行了一些扩展，比如`spawn`、`send`、`expect`和`interact`等

### spawn
```bash
spawn [args] <program> [args]
```
创建一个新的进程运行`program [args]`。`program`的标准输入、标准输出和标准错误都连接到`Expect`，这样程序就可以被`Expect`读写了（当以spawn启动一个进程时，进程描述符被赋给变量spawn_id，它表示了当前进程，spawn_id可被读写，以实现任务控制）

选项 | 说明
---|---
-noecho | 默认情况下，spawn会回显命令名和参数。此选项可以取消回显。
-ignore | 指定在spawned process种要忽略的信号。


### expect
```bash
expect [[-opts] pat1 body1] ... [-opts] patn [bodyn]
```
等待，直到模式`patn`匹配到`spawn`打开的进程的输出，或超过指定的时间，或遇到`EOF`
- 如果模式关键字是`eof`，那么对应的执行体是处理遇到文件结尾
- 如果模式关键字是`timeout`，那么对应的执行体是处理超时的 （如果没有指明`timeout`，则默认不会做任何动作。默认的超时时间是`10`秒，可以通过命令重设`set timeout 30`, 如果设置为-1，则意味着无限等待）
- 如果模式关键字是`default`，那么对应的执行体会处理超时或文件结尾
- 如果模式关键词是`null`，那么仅当匹配到一个单个ascii字符"0"时才会执行后面的执行体

注：
- 每次新的输出到达时，会按顺序比较每个模式，如果模式匹配到了，那么对应的执行体会被执行。如果有多个模式匹配，只执行第一个出现的
- 字串中可以使用`^`匹配起始串，用`$`匹配结尾串
- 在获得匹配后，程序的所有输出字串，截至匹配字串，被保存在变量`expect_out(buffer)`中，多至九个匹配子串分别被保存在变量`expect_out(1,string)`至`expect_out(9,string)`中

示例：
```bash
expect {
    "yes/no"       {send "yes\r";exp_continue}
    "password:"    {send "$passwd\r"}
    default        {exit}
}
```

选项 | 说明
---|---
-gl | 保护以"-"开始的模式不被认为是选项
-re | 正则表达式，在单项前面指定
-ex | 匹配确切的字串，不对*、^和$等特殊字符进行翻译
-nocase | 不区分大小写
-timeout | 设置当前expect的超时时间，而不是使用变量timeout的时间

### exp_continue
```bash
exp_continue [-continue_timer]
```
允许`expect`继续执行自身而不是往下执行，默认情况下，`exp_continue`会重置`timeout`，如果不想重置`timeout`，使用`-continue_timer`选项

### expect_user
```bash
expect_user [expect_args]
```
类似expect，不过是从标准输入读取字符，行必须以回车结尾，以使expect能识别它们

### send
```bash
send [-flags] <string>
```
发送string到当前进程

示例：
```bash
发送string到当前进程
```
发送`hello <blank> world <return>`到当前进程，`\r`表示回车键

选项 | 说明
---|---
-- | 其后的参数被强制解释成字串，而不是选项
-i | 选项说明字串发送给spawn_id
-s/-h | 慢的输入\类人的输入方式，可以提供输入的间隔设置

### send_error
```bash
send_error [-flags] <string>
```
类似send，不过输出发送到标准错误，而不是当前进程

### send_log
```bash
send_log [--] <string>
```
类似send，不过string只发送到log文件

### send_tty
```bash
send_tty [-flags] <string>
```
类似send，不过输出发送到/dev/tty而不是当前进程（在终端上显示提示符）

### send_user
```bash
send_user [-flags] string
```
类似send，不过输出发送到标准输出，而不是当前进程

### interact
```bash
interact [string1 body1] ... [stringn [bodyn]]
```
将当前进程的控制权交付给用户

`string-body`对可以作为参数，以使当有string输入时，执行body
示例：
```bash
interact {
    "abc" {send_user "you typed abc\n"}
    "123" {send_user "you typed 123\n"}
}
```
选项 | 说明
---|---
-ex | 防止以"-"开头的模式被翻译成选项
-re | 用正则匹配的模式翻译string，此选项下匹配的子串被保存在变量interact_out中。类似expect的expect_out。
-echo | 回显每一个字符，即使这个字符会被匹配中

- 当模式是`eof`时，表示遇到文件结尾的行为，默认是"return"。
- 当模式是`timeout`时（需指定超时时间），表示在指定时间没有输入时的行为，此项没有默认的超时时间，变量`timeout`的值在此无效。
- 当模式是`null`时，仅当匹配到一个单个ascii字符"0"时才会执行后面的执行体。没有可用的办法来匹配0字节数据，不论是通过全匹配和正则匹配。

`interact`中的`return`使`interact`返回到它的调用。而`inter_return`使`interact`调用执行`return`

### sleep
```bash
sleep <seconds>
```
脚本进入睡眠模式，睡眠时间单位为秒

### close
```bash
close [-slave] [-onexec 0|1] [-i spawn_id]
```
关闭连接到当前进程的连接

选项 | 说明
---|---
-i | 指定关闭名为spawn_id的进程
-onexec | 检测是否有新打开的进程或进程是否有重叠，若有，0：保持打开，1：强制关闭
-slave | 关闭spawn_id关联的子进程

### exit
Expect退出

## 内部变量
变量 | 含义
---|---
`$argv` | expect运行参数集合(非内部可选项)
`$argc` | `$argv`的长度
`$expect_out(buffer)` | 满足匹配的字符串中从起始位置到匹配字串。使用正则时，子项保存在`$expect_out(<n>,string)`中（n取1～9，0表示整个字符串）

注：
- 集合类变量可以通过`[lindex <var> <index> ]`取到指定项

- - -
**参考**：
- [Expect使用小记](https://www.cnblogs.com/yinghao1991/p/6926125.html)
- [Shell脚本学习之expect命令](https://www.cnblogs.com/lixigang/articles/4849527.html)
