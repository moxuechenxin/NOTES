## 命令
```bash
git log
```
## 参数／选项
**`git log`常用选项**：
选项 | 说明
---|---
`-p` | 按补丁格式显示每个更新之间的差异。
`--stat` | 显示每次更新的文件修改统计信息（列出了修改过的文件，以及其中添加和移除的行数，并在最后列出所有增减行数小计）
`--shortstat` | 只显示 `--stat` 中最后的行数修改添加移除统计。
`--name-only` | 仅在提交信息后显示已修改的文件清单。
`--name-status` | 显示新增、修改、删除的文件清单。
`--abbrev-commit` | 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。
`--relative-date` | 使用较短的相对时间显示（比如，“2 weeks ago”）。
`--graph` | 显示 ASCII 图形表示的分支合并历史。
`--pretty` | 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。

### `--pretty`
修改提交历史的展示格式  
**子选项**：
- `medium`：默认值
- `short`
- `full`
- `fuller`
- `email`
- `raw`
- `oneline`：将每个提交放在一行显示（`--online`也可以作为单独的属性））

```bash
git log --pretty=oneline
# git log --oneline 
```
- `format`：可以定制要显示的记录格式
```bash
git log --pretty=format:"%h - %an, %ar : %s"
# ca82a6d - Scott Chacon, 6 years ago : changed the version number
# 085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
# a11bef0 - Scott Chacon, 6 years ago : first commit
```
**`git log --pretty=format` 常用的选项**：
选项 | 说明
--- | ---
`%H` | 提交对象（commit）的完整哈希字串
`%h` | 提交对象的简短哈希字串
`%T` | 树对象（tree）的完整哈希字串
`%t` | 树对象的简短哈希字串
`%P` | 父对象（parent）的完整哈希字串
`%p` | 父对象的简短哈希字串
`%an` | 作者（author）的名字
`%ae` | 作者的电子邮件地址
`%ad` | 作者修订日期（可以用 `--date=` 选项定制格式）
`%ar` | 作者修订日期，按多久以前的方式显示
`%cn` | 提交者（committer）的名字
`%ce` | 提交者的电子邮件地址
`%cd` | 提交日期
`%cr` | 提交日期，按多久以前的方式显示
`%s` | 提交说明

**带颜色的`git log --pretty=format`**：
```bash
git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
```
断句：`%Cred%h` `%Creset -` `%C(yellow)%d` `%Creset %s` `%s %Cgreen(%cr)` `%C(bold blue)<%an>` `%Cresets`

说明：颜色以`%C`开头，设置字体属性要用括号`()`

- 颜色可选值：`reset`(默认的灰色)、`normal`、`black`、`red`、`green`、`yellow`、`blue`、`magenta`、`cyan`、`white`
- 字体属性：`bold`、`dim`、`ul`、`blink`、`reverse`
- 内容可以是占位元字符，也可以是直接显示的普通字符

### `--graph`
当`oneline`或`format`与`--graph`结合使用时，将添加一些ASCII字符串来形象地展示分支、合并历史

### `--date`
定制时间显示格式
**子选项**：
- `relative`：显示相对时间
```bash
git log --date=relative
# Date:   12 hours ago
```

- `local`
- `iso`
- `rfc`
- `short`：显示`YYYY-MM-DD`格式的时间
- `raw`：时间戳
- `default`
- `format`：格式化时间格式

**`git log --date=format` 格式化占位符**：
```bash
git log --date=format:'%Y-%m-%d %H:%M:%S'
# 2016-01-13 11:32:13
```
占位符 | 说明
---|---
%A | 
%a | 
%B | 月份全称
%b | 月份简称
%c | 
%d | 日
%H | 小时
%I | 
%j |
%M | 分
%m | 月
%p |
%S | 秒
%U |
%W |
%w |
%X |
%x | 
%Y | 年
%y | 
%Z, %z |
%% | 


## 限制输出的选项
选项 | 说明
---|---
`-(n)` | 仅显示最近的 n 条提交
`--since`, `--after` | 仅显示指定时间之后的提交
`--until`, `--before` | 仅显示指定时间之前的提交
`--author` | 仅显示指定作者相关的提交
`--committer` | 仅显示指定提交者相关的提交
`--grep` | 仅显示含指定关键字的提交
`-S` | 仅显示添加或移除了某个关键字的提交（`-G`用正则匹配）
`--no-merges` | 过滤掉merge commit
`--merges` | 只显示merge commit

- 时间限制

```bash
# 列出所有最近两周内的提交
git log --since=2.weeks

# 某一天的提交
git log --since="2018-01-15"
```
- 添加或移除了某些字符串

```bash
git log -S"\$_request"
```
只输出新增或删除了字符串`$_request`的提交（注意`$`前用`\`转义）

- 限制路径/文件

```bash
git log -- dir1/ dir2/file1.js
```
只输出`dir1`目录和`dir2/file1.js`文件的提交（放在最后位置上的选项，多个值间用空格分隔，路径支持正则。如果文件路径不会和分支名重复，可以省略`--`）

- 限制分支
```bash
## 当前分支下有个名为dev的文件，同时有一个名为dev的分支
git log dev -- # dev表示分支(--后为空)
git log -- dev # dev代表的文件
git log dev -- dev # 表示dev分支下的dev文件
```

- 限制分支范围
```bash
# 只显示在<branch2>但不在<branch1>的提交
git log <branch1>..<branch2> # 等同于 git log <branch2> --not <branch1> 

# 显示在<branch1>或<branch2>的提交
git log <branch1>...<branch2>
```

- 限制提交范围
```bash
# 查询commit之前的记录，包含commit
git log <commit>

# 查询<commit1>与<commit2>之间的记录（包含<commit1>和<commit2>）
git log <commit1> <commit2>

# 查询<commit1>与<commit2>之间的记录（不包含<commit1>）
git log <commit1>..<commit2>
```
<commit>可以是提交哈希值的简写模式，也可以使用`HEAD`代替  
`HEAD`代表最后一次提交，`HEAD^`为最后一个提交的父提交，等同于`HEAD～1`

- 限制tag标签
```bash
# 查询 v1.0之前的提交
git log v1.0
# 查询 v1.0之后的提交(不包含v1.0)
git log v1.0..
```

- 匹配作者author

```bash
git log --author="zhengjitf"
```

- 匹配commit说明中的关键字

```bash
# 可以用-i来忽略大小写
git log --grep="update"
```
只显示提交说明中有`update`字符的提交  

**注意**：如果想同时使用`--grep`和`--author`，必须在附加一个`--all-match`参数



**参考**：
- [git log命令全解析，打log还能这么随心所欲！](https://www.cnblogs.com/bellkosmos/p/5923439.html)
- [ProGit：2.3 Git 基础 - 查看提交历史](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2)