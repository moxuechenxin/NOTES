# 安装
# 配置
### 用户信息

```shell
git config --global user.name "zhengji"
git config --global user.email "email@example.com"
```
**--global** 选项表示全局设置，如果要在当前项目设置，则去掉 **--global** ，当前目录设置会覆盖全局设置。

删除相应配置用`--unset`参数  
查看配置用`-l`参数

### 文本编辑器

```shell
git config --global core.editor emacs
```

### alias
```
# 设置checkout的别名为co
git config --global alias.co checkout
# 设置log格式化输出别名为logs
git config --global alias.logs "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"
```
 
### 查看配置信息
- 列出所有git当时能找到的配置（重复配置项以最后一个为准）
```shell
git config --list
```
- 检查 Git 的某一项配置
```shell
git config <key>
```
### 设置代理（基于ShadowSocks代理）
**全局**
```shell
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080' 
```
**github**
```shell
git config --global http.https://github.com.proxy 'socks5://127.0.0.1:1080' 
```

# 帮助
```shell
git help <verb>
git <verb> --help
man git-<verb>
```
例如：`git help config`获取config命令的手册（本地文件）

# 基础
### 获取git仓库
#### 1. 本地项目
- **在项目目录初始化仓库**
```shell
git init
```
- **关联一个远程库**  
远程仓库默认名称为 origin
```shell
git remote add origin git@server-name:path/repo-name.git
```

#### 2.克隆远程仓库
clone的远程仓库默认以origin命名 (`-o <remote-name>` 指定远程仓库名称) fileName表示克隆本地的仓库文件夹名称，不指定默认同远程
```shell
git clone git@server-name:path/repo-name.git [fileName] [-o <remote-name>]
```

### 记录每次更新到仓库
#### 检查当前文件状态
```shell
git status
```
#### 跟踪新文件
```shell
# 跟踪单个（类）文件（可使用通配符）
git add <file>
# 跟踪全部文件
git add .
```
#### 暂存已修改文件
命令同'**跟踪新文件**'

#### 忽略文件
在仓库目录创建.gitignore文件  

**文件 .gitignore 的格式规范如下**：
- 所有空行或者以 ＃ 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式匹配。
- 匹配模式可以以（/）开头防止递归。
- 匹配模式可以以（/）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

所谓的 glob 模式是指 shell 所使用的简化了的正则表达式:   
- 星号（\*）匹配零个或多个任意字符；
- [abc] 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；  
- 问号（?）只匹配一个任意字符；  
- 如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如[0-9] 表示匹配所有 0 到 9 的数字）。  
- 使用两个星号（\*) 表示匹配任意中间目录，比如`a/**/z` 可以匹配 a/z,a/b/z 或 `a/b/c/z`等。

```bash
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```

#### 查看已暂存和未暂存的修改
##### 1. 当前文件和暂存区域快照之间的差异(也就是修改之后还没有暂存起来的变化内容)

```shell
git diff
```
##### 2. 查看已暂存的将要添加到下次提交里的内容
```shell
git diff --canched
```
（Git 1.6.1 及更高版本
还允许使用 git diff --staged)  

**注意：** git diff 本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动

##### 3. 图形化输出
使用 `git difftool` 命令来用 Araxis ，emerge 或 vimdiff 等软
件输出 diff 分析结果。  
使用 `git difftool --tool-help` 命令来看你的系统支持哪些 Git
Diff 插件
#### 提交更新
```shell
git commit [-v]
```
这种方式会启动文本编辑器以便输入本次提交的说明, **-v** 选项，这会将你所做的改变的 diff 输出放到编辑器中从而使你知道本次提交具体做了哪些修改

```shell
git commit -m "提交信息"
```
**-m** 选项，将提交信息与命令放在同一行

#### 跳过使用暂存区域
```shell
git commit -a
```
给 `git commit` 加上 `-a` 选项，Git 就会自动把所有**已经跟踪过的文件**暂存起
来一并提交，从而跳过 `git add` 步骤

#### 移除文件
- 删除文件并从暂存区移除
```shell
git rm <file>
```
如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 `-f`(数据不能被 Git 恢复)

- 把文件从 Git仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中
```shell
git rm --cached <file>
```
#### 移动文件(重命名)
```shell
git mv <oldName> <newName>
```
此命令相当于运行了下面三条命令
```shell
mv <oldName> <newName>
git rm <oldName>
git add <newName>
```

### 查看提交历史
查看当前分支提交历史
```shell
git log 
```
`--all`选项可查看所有分支提交历史  
`-<num>`显示最近的几条记录

各种配置选项，请查看文档

### 撤消操作
#### 1. 重新提交
```shell
git commit --amend [-m <message>]
```
使用一次新的commit，替代上一次提交  
如果暂存区文件没有任何新变化，则用来改写上一次commit的提交信息
#### 2. 取消暂存的文件
```shell
git reset HEAD <file>
```
#### 3. 撤消对文件的修改
```shell
git checkout -- <file>
```
**注意**： 这是一个危险的命令，对那个文件做的任何修改都会消失

### 远程仓库的使用
#### 查看远程仓库
```shell
git remote [-v]
```
选项 `-v`，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL

查看某一个远程仓库的更多信息
```shell
git remote show [remote-name]
```
该命令会列出远程仓库的 URL 与跟踪分支的信息
#### 添加远程仓库
```shell
git remote add <shortname> <url>
```

#### 推送到多个分支
```shell
git remote add <repos-name> <url-1>   
git remote set-url --add <repos-name> <url-2>   
```
推送时:
```
git push <repos-name> --all
```

#### 远程仓库的移除与重命名
- 重命名
```shell
git remote rename <old> <new>
```
**注意**：这同样也会修改你的远程分支名字。那些过去引用 `<old>/master` 的现在会引用 `<new>/master`。

- 移除
```shell
git remote rm <remote-name>
```

#### 从远程仓库中抓取与拉取
- 获取fetch
```shell
git fetch [remote-name]
```
这个命令会访问远程仓库，从中拉取所有你还没有的数据。执行完成后，你将会拥有那个远程仓库中所有分支的
引用，可以随时合并或查看。

- 拉取pull
```shell
git pull [remote-name [branch-name] ]
```
`git pull` 命令来自动的抓取然后合并远程分支到当前分支

#### 推送到远程仓库
```shell
git push [remote-name] [branch-name]
```

### 打标签
#### 列出标签
```shell
git tag
```
列出特定模式标签
```shell
git tag -l 'v1.8.5*'
＃v1.8.5
＃v1.8.5-rc0
＃v1.8.5-rc1
＃v1.8.5-rc2
＃v1.8.5-rc3
＃v1.8.5.1
```
#### 创建标签
两种主要类型：**轻量标签**（lightweight）和 **附注标签**（annotated）

- **轻量标签**
轻量标签本质上是将提交校验和存储到一个文件中，没有保存任何其他信息，默认将标签打在最新一次提交上
```shell
git tag v1.4-lw

git tag
# v0.1
# v1.3
# v1.4
# v1.4-lw
# v1.5
```
- **附注标签**
```shell
git tag -a v1.4 -m 'my version 1.4'
```
`-m` 选项指定了一条将会存储在标签中的信息。如果没有为附注标签指定一条信息,Git 会运行编辑器要求你输入信息

通过使用 `git show` 命令可以看到标签信息与对应的提交信息
```shell
git show v1.4
```
- **后期打标签**
```shell
git tag -a v1.2 <commit-id>
```
- **共享(推送)标签**
默认情况下,`git push` 命令并不会传送标签到远程仓库服务器上。在创建完标签后你必须显式地推送标签到共享服务器上
```shell
git push origin <tagname>
```
推送所有标签
```shell
git push origin --tags
```
- **检出标签**

### Git 别名(命令快捷方式)
```shell
git config --global alias.<alias-name> '<git-command>'
```
想要执行外部命令,而不是一个 Git 子命令,可以在命令前面加入 ! 符号
```shell
git config --global alias.<alias-name> '!<git-command>'
```

## Git分支
### 分支简介
#### 创建分支
```shell
git branch <branch-name> [<base-branch>]
```
基于当前分支（或特定分支）创建新分支
```shell
git log --oneline --decorate
```
`--decorate`参数可查看各个分支当前所指的对象

#### 切换分支
```shell
git checkout <branch-name>
```
#### 创建并切换到新分支
```shell
git checkout -b <new-branch-name>
```
### 分支的新建与合并
#### 将某分支合并到当前分支
```shell
git merge <branch-name>
```
生成合并提交
`--no-commit` 选项在默认合并过程中可以用来延迟(即仅合并，需手动提交)
`--squash` 选项接受被合并的分支上的所有工作，并将其压缩至一个变更集，使仓库变成一个真正的合并发生的状态，而不会真的生成一个合并提交
#### 删除分支
```shell
git branch -d <branch-name>
```
未被合并的分支需执行`git branch -D <branch-name>`删除（强制删除）
#### 遇到冲突时的分支合并
在合并冲突后的任意时刻使用 `git status` 命令来查看那些因包含合并冲突而处于未合并（unmerged）状态的文件

**放弃合并**
```shell
git merge --abort
```

使用图形化工具解决冲突
```shell
git mergetool
```

解决冲突后，使用 `git add` 将其标记为冲突已解决

使用 `git commit` 完成合并提交

### 分支管理
#### 查看所有分支
```shell
git branch
```
查看每个分支的最后一次提交
```shell
git branch -v
```
`--merged` 与 `--no-merged`  选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支

### 分支开发工作流
#### 长期分支
#### 特性分支

### 远程分支
显式地获得远程引用的完整列表
```shell
 git ls-remote <remote-name>
```
获得远程分支的更多信息
```shell
 git remote show <remote-name>
```
#### 远程跟踪分支
远程跟踪分支是远程分支状态的引用,远程跟踪分支像是你上次连接到远程仓库时，那些分支所处状态的书签,以 `<remote-name>/<branch-name>` 形式命名

#### 推送
```shell
  git push <remote-name> <branch-name>
```

将本地分支推送到远程另一个不同命名的分支
```shell
  git push <remote-name> <local-branch-name>:<remote-branch-name>
```
**注意**：当抓取到新的远程跟踪分支时，本地不会自动生成一份可编辑的副本（拷贝）， 只有一个不可以修改的远程分支指针，可以运行 `git merge origin/<remote-branch-name>` 将这些工作合并到当前所在的分支,如果想要在新建分支上工作，可以将其建立在远程跟踪分支之上`git checkout -b <new-branch-name> <remote-name>/<remote-branch-name>`,这会给你一个用于工作的本地分支，并且起点位于 `<remote-name>/<remote-branch-name>`。

#### 跟踪分支
- 创建一个跟踪远程分支的新分支（同名）
```shell
git checkout --track <remote-name>/<remote-branch-name>
```
- 创建一个跟踪远程分支的新分支（不同名）
```shell
git checkout -b <new-branch-name> <remote-name>/<remote-branch-name>
```
- 设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支,使用`-u` 或`--set-upstream-to` 选项
```shell
git branch -u <remote-name>/<remote-branch-name>
```
##### 查看所有的跟踪分支
```shell
git branch -vv
```
想要统计最新的领先与落后数字
```shell
git fetch --all
git branch -vv
```
#### 拉取
```shell
git pull [remote-name] [remote-branch-name]
```
#### 删除远程分支
```shell
git push [remote-name] --delete [remote-branch-name]
```
基本上这个命令做的只是从服务器上移除这个指针

### 变基
#### 变基的基本操作
将当前分支变基到另外一个分支
```shell
git rebase <branch-name>
```
切换到变基目标分支，然后合并变基的分支(快速合并)
```shell
git checkout <another-branch-name>
git merge <rebased-branch-name>
```
结果和普通合并（三方合并）相同

#### 变基高级操作 
```shell
git rebase --onto <to-branch-name> <another-branch-name> <rebase-branch-name>
```
取出 `<rebase-branch-name>` 分支,找出处于`<rebase-branch-name>`分支和`<another-branch-name>`分支的共同祖先之后的修改,然后把它们在 `<to-branch-name>`分支上重演一遍(即变基到该分支)。然后快速合并，命令同基本操作

```shell
git rebase <to-branch-name> <rebase-branch-name>
```
将`<rebase-branch-name>`变基到`<to-branch-name>`。然后快速合并，命令同基本操作

变基合并到主分支后，可删除变基分支

#### 变基的风险
**变基的准则**：
**不要对在你的仓库外有副本的分支执行变基。**

#### 用变基解决变基

## 分布式Git
### 分布式工作流程
- **集中式工作流**
- **集成管理者工作流**
- **司令官与副官工作流**

### 向一个项目贡献
```shell
 git diff --check
```
检查空白错误（行尾的空格、Tab 制表符，和行首空格后跟 Tab 制表符的行为）

```shell
git push -u <remote-name> <local-branch>:<remote-branch>
```
`-u`是`--set-upstream`的简写，加上`:`表示将本地某分支推送到远程某分支

```shell
git log <branch1>..<branch2>
```
`..`是一种日志过滤器。表示只显示在`<branch2>`但不在`<branch1>`的提交的列表，等同于以下命令
```shell
git log <branch2> --not <branch1>
```

### 维护项目
#### 在特性分支中工作
#### 应用来自邮件的补丁

##### 使用 apply 命令应用补丁
应用使用 `git diff` 或 Unix diff命令（不推荐）创建的补丁
```shell
git apply path/to/*.patch
```
`git apply` 命令采用了一种“全部应用，否则就全部撤销（apply all
or abort all）”的模型，即补丁只有全部内容都被应用和完全不被应用两个状态(不会创建提交)

```shell
git apply --check
```
在实际应用补丁前,检查补丁是否可以顺利应用

##### 使用 am 命令应用补丁
应用一个由 `format-patch` 命令生成的补丁
```shell
git am path/to/*.patch
```
(会创建新提交)

#### 检出远程分支
#### 确定引入了哪些东西
```shell
git log -p
```
查看每次提交所引入的具体修改,该命令后会在每次提交
后面附加对应的差异（diff）

**查看将该特性分支与另一个分支合并的完整 diff**

- 特性分支与直接祖先分支diff（未产生分叉）
```shell
git diff <base-branch>
```
- 产生了分叉
```shell
git merge-base <current-branch> <another-branch>
```
或者
```shell
git merge-base <another-branch>...<current-branch> 
```
对于 `diff` 命令来说，你可以通过
把 `...` 置于另一个分支名后来对该分支的最新提交与两个分支的共同祖先进行比较

#### 将贡献的工作整合进来

**参考：**
- [ProGit（简体中文）](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%85%B3%E4%BA%8E%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6)
