## 检出
```bash
# svn co
svn checkout <remote-url> [<local-path>] [--username <username>] [--password <password>]
```
不指定本地目录全路径`<local-path>`，则检出到当前目录下

## 导出(不带.svn文件夹)
```bash
svn export [-r <版本号>] <remote-path> [<local-path>] [--username <username>] [--password <password>]
```

## 添加到版本控制
```bash
svn add [--force] <filename>
```
文件名`<filename>`支持glob匹配

#### 只添加一个目录而不包括其内容
```bash
svn add --depth=empty <dirname>
```

#### 一次性增加所有新增的文件到svn库：
```bash
svn st | awk '{if ($1 == "?") {print $2} }' | xargs svn add
```


## 提交到服务器
```bash
# git ci
# 如果选择了保持锁，就是用-no-unlock开关
svn commit -m <commit-msg> [-N] [--no-unlock] <commit-path>
```

## 更新本地仓库
```bash
svn update [-r <修正版本>] <local-path>
```

## 删除文件
```bash
svn delete <remote-path> [-m <commit-msg>]
```

#### 一次性从svn库删除所有需要删除的文件
```bash
svn st | awk '{if ($1 == "!") {print $2}}' | xargs svn rm
```

## 加锁/解锁
```bash
# 加锁
svn lock -m <commit-msg> [--force] <local-path>

# 解锁
svn unlock <local-path>
```

## 比较差异
参考：[【svn】svn的diff命令用法详解](https://blog.csdn.net/dong123dddd/article/details/52451190)
```bash
svn diff [-r <修正版本号1>:<修正版本号2>] <local-path>
```

## 查看目录状态
```bash
# svn st
svn status [<local-path>] 
```
状态符：
- `?`：不在svn的版本控制中
- `M`：内容被修改
- `C`：发生冲突
- `A`：预定加入到版本库
- `K`：被锁定

## 查看日志
```bash
# 显示这个目录(文件)的所有修改记录，以及版本号的变化
svn log <path>
```

## 查看文件详细信息
```bash
svn info <path>
```

## 查看版本库下的文件和目录列表
```bash
# svn ls
svn list <remote-path>
```

## 撤销本地修改
```bash
svn revert [--recursive/-r] <file/dir>
```

## 修改远程仓库地址
```bash
svn switch <remote-path> <local-path>
```

## 解决冲突
```bash
svn resolved <local-path>
```
## 设置忽略
```bash
# 在项目根目录执行
# 指定svn编辑器：在`~/.bash_profile`加入`export SVN_EDITOR=vim`
svn propedit svn:ignore .
```

- - -
**参考**：
- [svn 常用操作命令](https://www.cnblogs.com/liwanggui/archive/2016/06/28/5624147.html)
