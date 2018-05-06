### git status 显示中文

```js
git config --global core.quotepath false
```

### [合并分支, 当冲突时，保持某些文件不被合并](http://blog.csdn.net/fkaking/article/details/44955663?ref=myread)
1. 创建自定义merge driver
```bash
git config --global merge.ours.driver true 
```
2. 在要被merge的分支上创建.gitattributes 文件，并且在文件中置顶不merge的文件名
```bash
echo 'email.json merge=ours' >> .gitattributes  
git add .gitattributes  
git commit -m 'chore: Preserve email.json during merges' 
```
3. 回到要合并到的分支（注意形容词）执行merge
```bash
git checkout master  
git merge newbranch  
# Auto-merging ...  
# Merge made by the 'recursive' strategy.  
# demo-shared | 1 +  
# 1 file changed, 1 insertion(+)
```
经过以上步骤，我们指定的email.json就不会被合并

### git只clone仓库中指定子目录和指定文件

```bash
git init demo
cd demo

git config core.sparsecheckout true
echo '/path/*' >> .git/info/sparse-checkout
git remote add origin <remote-url>
git pull origin
```

### 更新特定目录或文件
```bash
git fetch
# -m 表示 --merge
git checkout -m <版本号> <文件／文件夹>
# 或
git checkout origin/master -- <path/to/file>
```
### 合并指定分支的指定文件
```bash
git checkout <another-branch> <path/to/file>
```

### [用 Git Subtree 在多个 Git 项目间双向同步子项目，附简明使用手册](https://segmentfault.com/a/1190000003969060)

### 查看某文件的修改历史
```bash
# 推荐
# 查看指定提交的指定文件的修改
git show <commit-id> <file>
```

```bash
# 查看指定提交id之前修改了指定文件的提交历史（按补丁格式显示每个更新之间的差异）
git log -p <commit-id> -- <file>
```
