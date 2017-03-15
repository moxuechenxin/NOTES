- #### 设置用户名和邮箱(--global表示全局)
    \- git config --global user.name "Your Name"  
    \- git config --global user.email "email@example.com"
- #### 查看用户名和邮箱
    \- git config --global user.name  
    \- git config --global user.email  
    \- git config --global --list  
    \- 去掉global参数，即为本项目配置
- #### 创建命令快捷方式
`git config --global alias.<alias-name> <git-command>`  
例如：git config --global alias.pom 'push origin master'
- #### git bash 配置
    \- **保存认证**：git config --global credential.helper store  
    \- **忽略ssl证书**：git config --global http.sslVerify false  
    \- **配置空目录**：git config --bool core.bare true
- #### 创建仓库
    \- git init
- #### 将文件添加到暂存区
    \- git add example.txt  
    \- git add .  将修改的文件全部添加到暂存区
- #### 把文件提交到版本库
    \- git commit -m "提交说明"  
    \- 注意：要先将修改文件放入暂存区（add），然后再提交到工作区（commit）
- #### 替换上次提交
    - ##### git commit --amend -m [message]
        \- 使用一次新的commit，替代上一次提交  
        \- 如果代码没有任何新变化，则用来改写上一次commit的提交信息  
    - ##### git commit --amend [file1] [file2] ...
        \- 重做上一次commit，并包括指定文件的新变化
- #### 将修改文件放入暂存区后提交到版本库
    \- git commit -a -m "提交说明"
- #### 对比文件差异
    - ##### 工作区和暂存区的差异
        \- git diff example.txt  
    - ##### 暂存区和版本库的差异
        \- git diff --cached(--staged) example.txt
    - ##### 工作区和版本库的差异
        \- git diff branch_zj example.txt
    - ##### 本地和远程版本的差异
        \- git diff branch_zj origin/branch_zj  
- #### 查看工作区和暂存区状态
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
    \- git reset --hard HASH 返回到某个节点，不保留修改。  
    \- git reset --soft HASH 返回到某个节点。保留修改。
- #### 撤销修改
    - ##### 修改后 未add（添加到暂存区） 需要撤销修改时：  
        \- git checkout -- example.txt 或 手动删除工作区修改  
        \- git checkout .   (撤销所有修改)
        \- 工作区 ： clean  暂存区： clean
    - ##### 修改后 add了（未commit） 再次修改文件  要撤销第二次修改时：  
        \- git checkout -- example.txt (销毁工作区修改)  
        \- 暂存区有第一次的修改需要commit  
    - ##### 修改后 add了（未commit），需要撤销修改时：  
        \- git reset HEAD example.txt (将暂存区恢复到工作区)  
        \- 此时工作区的修改还未撤销  
        \- git checkout -- example.txt (撤销工作区修改)  
    - ##### 修改后 add并commit了，需要撤销修改时：  
        \- git reset --hard HEAD^  (版本回退)  
        \- git checkout [commit_id] [file] 恢复某个commit的指定（全部）文件到暂存区和工作区

- #### 删除文件
    - ##### 将文件放入暂存区并提交
        \- git add example.txt  
        \- git commit -m "new commit"
    - ##### 删除文件
        \- rm example.txt
    - ##### 情况一：确定要从版本库中删除该文件
        \- git rm example.txt  
        \- git commit -m "remove example.txt"
    - ##### 情况二：恢复误删
        \- git checkout -- example.txt  
        \- git checkout其实是用版本库里的版本替换工作区的版本,只能恢复文件到提交的最新版本
    - ##### 同时删除工作区和暂存区的同一文件
        \- git rm -f example.txt
    - ##### 只删除暂存区文件，保留工作区文件
        \- git rm --cached example.txt
- #### 添加远程库
    - ##### 关联一个远程库
        \- git remote add origin git@server-name:path/repo-name.git  
        \- 远程仓库默认名称为 origin
    - ##### 查看远程库信息
        \- git remote  
        \- git remote -v 显示详细信息
    - ##### 删除远程库
        \- git remote remove origin  
        \- origin为默认仓库名
    - ##### 推送本地内容
        \- git push -u origin master  
  
        \- 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令  
  
        \- 若提示要先fetch，则执行git pull origin master 
  
        \- 成功后提示输入描述为什么合并是必须的，操作如下：    
        　 press "i"  
        　 write your merge message  
        　 press "esc"  
        　 write ":wq"  
        　 then press enter    
        　   
        \- 然后再执行以上操作
- #### 创建与合并分支
    \- **查看分支**：git branch  

    \- **创建分支**：git branch <name>    

    \- **切换分支**：git checkout <name>

    \- **创建+切换分支**：git checkout -b <name>

    \- **合并某分支到当前分支**：git merge <name>

    \- **删除分支**：git branch -d <name>
    
    \- **强制删除分支**：git branch -D <name>
    
    \- **查看当前分支合并的分支**： git branch --merged
    
    \- **查看当前分支没有合并的分支**： git branch --no-merged
- #### 解决冲突
    \- branch_zj分支对example.txt做了修改，add和commit后，切换到master分支  
    \- 同样在master分支修改example.txt，add和commit后，没有这一步一般不会冲突    
    \- 执行：git merge branch_zj  
    \- 提示冲突,修改文件  
    \- 再执行add和commit  
    \- git branch -d branch_zj 删除被合并的分支
- #### 强制禁用Fast forward模式
    \- git merge --no-ff -m "commit messagee" branch_zj
- #### 储藏当前工作区修改
    - #####  git stash
        \- 把当前分支所有没有 commit 的代码先暂存起来 
    - ##### git stash list
        \- 列出暂存记录
    - ##### git stash apply [stash_id]
        \- 恢复暂存代码,如果省略 stash_id 默认恢复最近的暂存记录
    - ##### git stash drop [stash_id]
        \- 删除指定暂存记录，若没指定stash_id则删除最近一条stash记录
    - ##### git stash pop [stash_id]
        \- 还原代码，并删除该条stash记录
    - ##### git stash clear
        \- 就是清空所有暂存区的记录  
- #### 推送分支
    \- git push origin <branch_name>  
    \- git push -f origin <branch_name>  (强制推送)
- #### 创建标签
    - ##### git tag <tag_name>
        \- 默认标签是打在最新提交的commit上  
    - ##### git tag <tag_name> <commit_id>
        \- 为指定的提交创建标签
    - ##### git tag
        \- 查看标签列表，标签是按字母排序的
    - ##### git show <tag_name>
        \- 查看标签信息
    - ##### git tag -a <tag_name> -m "description"
        \- 创建带说明的标签   
- #### 操作标签
    - ##### git tag -d <tag_name>
        \- 删除标签
    - ##### git push origin <tag_name>
        \- 推送某个标签到远程  
        \- 创建的标签都只储存在本地，不会自动推送到远程
    - ##### git push origin --tags
        \- 一次性推送全部尚未推送到远程的本地标签
    - ##### 删除远程标签
        \- git tag -d <tag_name> 本地删除  
        \- git push origin :refs/tags/<tag_name> 远程删除
- #### 忽略特殊文件
    - 添加.gitignore，写入要忽略的文件名
    - 忽略的文件不能执行add，可用 -f 强制添加到Git 
    - git add -f ignored.txt
    - 检查文件是否被忽略: git check-ignore -v ignored.txt
    