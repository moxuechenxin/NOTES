- #### 设置用户名和邮箱(--global表示全局)
    \- git config --global user.name "Your Name"  
    \- git config --global user.email "email@example.com"
- #### 创建仓库
    \- git init
- #### 将文件添加到仓库
    \- git add example.txt
- #### 把文件提交到仓库
    \- git commit -m "提交说明"
- #### 查看文件修改记录
    \- git diff example.txt
- #### 查看暂存区状态
    \- git status
- #### 查看提交日志
    \- git log  
    \- 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数
- #### 版本回退
    \- git reset --hard HEAD^  
    \- 用HEAD表示当前版本，HEAD\^表示上一个版本，HEAD^^表示上上一个版本，往上100个版本用HEAD~100表示  
    \- git reset --hard commit_id  
    \- 回到commit_id代表的某个版本  
    \- 以上操作会删除git log记录
- #### 撤销暂存区的修改（unstage），重新放回工作区
    \- git reset HEAD example.txt
- #### 查看操作命令记录
    \- git reflog
- #### 撤销文件修改操作到上一次提交或暂存
    \- git checkout example.txt  
    \- 让文件回到最近一次git commit或git add时的状态
- #### 提交后，查看工作区和版本库里面最新版本的区别
    \- git diff HEAD -- example.txt
- #### git branch a 
  新建一个名为a的分支
- #### git checkout -b a
  这个命令的意思就是新建一个a分支，并且自动切换到a分支。　　
- #### git branch -d a
  删除分支a
- #### git branch -D a
  强制删除分支a
- #### git stash
  把当前分支所有没有 commit 的代码先暂存起来
- #### git stash list
  列出暂存记录
- #### git stash apply
  恢复暂存代码
- #### git stash drop [stash_id]
  删除指定暂存记录，若没指定stash_id则删除最近一条stash记录
- #### git stash pop
  还原代码，并删除该条stash记录
- #### git stash clear
  就是清空所有暂存区的记录