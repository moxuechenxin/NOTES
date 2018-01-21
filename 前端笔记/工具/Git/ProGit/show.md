## 命令
```bash
git show
```
一般使用此命令来显示一个标签或一个提交的信息

```bash
# 输出v1.4标签的信息
git show v1.4

# 查看某次提交的文件信息(包含文件的详细修改信息)
git show <commit>
# 查看仓库中 HEAD 在五次前的所指向的提交(git reflog 来查看引用日志)
git show HEAD@{5}
# 查看仓库中 HEAD 在两个月前所指向的提交
git show HEAD@{2.months.ago}
# 显示昨天master分支的顶端指向的提交(只对还在引用日志里的数据有用)
git show master@{yesterday}

# 查看某分支最近的提交文件信息(包含文件的详细修改信息)
git show <branch>
```