
## 初始化仓库
```bash
# 在当前目录新建一个Git代码库
git init

# 新建一个目录，将其初始化为Git代码库
git init [project-name]

# 下载一个项目和它的整个代码历史
git clone [url]
```

## 配置Git
Git的设置文件为 .gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）
```bash
# 显示当前的Git配置
git config --list

# 编辑Git配置文件
git config -e [--global]

# 设置提交代码时的用户信息
git config [--global] user.name "[name]"
git config [--global] user.email "[email address]"
```

## 增加删除文件
```bash
# 添加指定文件到暂存区
git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
git add [dir]

# 添加当前目录的所有文件到暂存区
git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
git add -p

# 删除工作区文件，并且将这次删除放入暂存区
git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
git mv [file-original] [file-renamed]
```

## 清理(删除)未跟踪文件
```bash
# 移除没有忽略的未跟踪文件（任何与 .gitignore 或其他忽略文件中的模式匹配的文件都不会被移除）
git clean

# 移除包含.gitignore匹配到的未跟踪文件
git clean -x

# 模拟执行（不会真正操作）
git clean -n

# 以交互方式运行
git clean -i

# 移除工作目录中所有未追踪的文件以及空的子目录（-f表示强制）
git clean -f -d

```

## 代码提交
```bash
git commit -m [message]

# 提交暂存区的指定文件到仓库区
git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
git commit -a

# 提交时显示所有diff信息
git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
git commit --amend [file1] [file2] ...
```

## 分支命令
```bash
# 列出所有本地分支
git branch

# 列出所有远程分支
git branch -r

# 列出所有本地分支和远程分支
git branch -a

# 新建一个分支，但依然停留在当前分支
git branch [branch-name]

# 新建一个分支，并切换到该分支
git checkout -b [branch]

# 新建一个分支，指向指定commit
git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
git checkout [branch-name]

# 切换到上一个分支
git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
git merge [branch]

# 选择一个commit，合并进当前分支
git cherry-pick [commit]

# 删除分支
git branch -d [branch-name]

# 删除远程分支
git push origin --delete [branch-name]
git branch -dr [remote/branch]
```

## 标签命令
```bash
# 列出所有tag
git tag

# 新建一个tag在当前commit
git tag [tag]

# 新建一个tag在指定commit
git tag [tag] [commit]

# 删除本地tag
git tag -d [tag]

# 删除远程tag
git push origin :refs/tags/[tagName]

# 查看tag信息
git show [tag]

# 提交指定tag
git push [remote] [tag]

# 提交所有tag
git push [remote] --tags

# 新建一个分支，指向某个tag
git checkout -b [branch] [tag]
```

## 查看信息
```bash
# 显示有变更的文件
git status

# 显示当前分支的版本历史
git log

# 显示commit历史，以及每次commit发生变更的文件
git log --stat

# 搜索提交历史，根据关键词
git log -S [keyword]

# 显示某个commit之后的所有变动，每个commit占据一行
git log [tag] HEAD --pretty=format:%s

# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
git log [tag] HEAD --grep feature

# 显示某个文件的版本历史，包括文件改名
git log --follow [file]
git whatchanged [file]

# 显示指定文件相关的每一次diff
git log -p [file]

# 显示过去5次提交
git log -5 --pretty --oneline

# 显示所有提交过的用户，按提交次数排序
git shortlog -sn

# 显示指定文件是什么人在什么时间修改过
git blame [file]

# 显示暂存区和工作区的差异
git diff

# 显示暂存区和上一个commit的差异
git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
git diff HEAD

# 显示两次提交之间的差异
git diff [first-branch]...[second-branch]

# 显示今天你写了多少行代码
git diff --shortstat "@{0 day ago}"

# 显示某次提交的元数据和内容变化
git show [commit]

# 显示某次提交发生变化的文件
git show --name-only [commit]

# 显示某次提交时，某个文件的内容
git show [commit]:[filename]

# 显示当前分支的最近几次提交
git reflog
```

## 远程同步
```bash
# 下载远程仓库的所有变动
git fetch [remote]

# 显示所有远程仓库
git remote -v

# 显示某个远程仓库的信息
git remote show [remote]

# 增加一个新的远程仓库，并命名
git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
git pull [remote] [branch]

# 上传本地指定分支到远程仓库
git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
git push [remote] --force

# 推送所有分支到远程仓库
git push [remote] --all
```

## 查看旧版本
```bash
# 签出一条以commit-id命名的临时commit分支
git checkout <commit-id>
```

## 撤销
```bash
# 恢复暂存区的指定文件到工作区
git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
git stash
git stash pop
```


#### 设置用户名和邮箱(--global表示全局)
```bash
git config --global user.name "Your Name"  
git config --global user.email "email@example.com"
```
#### 创建仓库
```bash
git init
```
#### 将文件添加到仓库
```bash
git add example.txt
```
#### 把文件提交到仓库
```bash
git commit -m "提交说明"
```
#### 查看文件修改记录
```bash
git diff example.txt
```
#### 查看暂存区状态
```bash
git status
```
#### 查看提交日志
```bash
# 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数
git log
```
#### 版本回退
```bash
# 用HEAD表示当前版本，HEAD\^表示上一个版本，HEAD^^表示上上一个版本，往上100个版本用HEAD~100表示 
git reset --hard HEAD^ 
# 回到commit_id代表的某个版本 
git reset --hard commit_id 
# 以上操作会删除git log记录
```
#### 撤销暂存区的修改（unstage），重新放回工作区
```bash
git reset HEAD example.txt
```
#### 查看操作命令记录
```bash
git reflog
```
#### 撤销文件修改操作到上一次提交或暂存
```bash
# 让文件回到最近一次git commit或git add时的状态
git checkout example.txt 
```
#### 提交后，查看工作区和版本库里面最新版本的区别
```bash
git diff HEAD -- example.txt
```
#### git branch a 
  新建一个名为a的分支
#### git checkout -b a
  这个命令的意思就是新建一个a分支，并且自动切换到a分支。　　
#### git branch -d a
  删除分支a
#### git branch -D a
  强制删除分支a
#### git stash
  把当前分支所有没有 commit 的代码先暂存起来
#### git stash list
  列出暂存记录
#### git stash apply
  恢复暂存代码
#### git stash drop [stash_id]
  删除指定暂存记录，若没指定stash_id则删除最近一条stash记录
#### git stash pop
  还原代码，并删除该条stash记录
#### git stash clear
  就是清空所有暂存区的记录