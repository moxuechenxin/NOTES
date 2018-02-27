## 常见错误
[`SVN Skipped ‘xxx’ — Node remains in conflict`](http://www.xuebuyuan.com/2169924.html)

解决办法:
```
svn remove --force filename
svn resolve --accept=working  filename
svn update
```
