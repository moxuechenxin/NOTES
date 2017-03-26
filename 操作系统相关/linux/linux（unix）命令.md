#### 当前目录新建文件
```shell
echo "文件内容" > <fileName>
```
#### 删除文件
```shell
rm <fileName>
```
#### 文件重命名
```shell
mv <oldName> <newName>
```
#### 新建文件
```shell
touch <fileName>
```
或
```shell
><fileName>
```
#### 查看文件内容
```shell
cat <fileName>
```

#### git bash的vim模式
- 输入信息
```shell
i
```
- 退出vim界面
```shell
:q 或者 :q!
```

## 目录与文件的基本操作
#### 查看你当前的位置：`pwd`

#### 查看文件夹：`ls`
默认列出当前目录下非隐藏的文件和文件夹  

查看指定目录下的文件和文件夹，`ls <path>`，可使用想对路径或绝对路径；  
mac平台查看相信信息，加上`-l`参数；  
查看隐藏文件和文件夹，加上`-a`参数；   
连同子目录内容一起列出（递归列出），加上`-R`参数


#### 清空命令行：`clear`
windows平台也可使用`cls`;  
mac可使用快捷键`command + K`

#### 路径中的符号
- 用户主目录`~`
- 根目录`/`
- 当前目录`./`，一般可省略
- 上一级目录`../`

#### 进入某个目录下：`cd`
`cd <path>`,可以是想对路径或绝对路径

**技巧**：  
- 路径可使用`tab`键补齐，如果匹配多个目录则按两次，输出目录可选项
- `cd`不加路径，直接回到主目录

#### 新建目录：`mkdir`
`mkdir <directory-name>`

#### 新建文件：`touch`
`touch <file-name>`

**创建目录结构**：  
windows：可以直接在 mkdir 命令的后面加上要创建的目录的路径，这个路径上面的所有的目录，如果还不存在 ，就会去创建一个  
mac：需要加上`-p`参数  
`mkdir -p path/to/directory`

#### 删除目录与文件：`rm`
- 删除文件
`rm <file> [ <another-file>... ]`  
``
- 删除目录  
`rm -r <directory> [ <another-directory>... ]`  
可以加上`-f`参数，这样在删除目录里的文件的时候不会出现提示, Windows Powershell不需要使用参数  

#### 移动（或重命名）文件夹或文件：`mv`
`mv <from-path> <to-path>`  
如果`<to-path>`不存在，则将`<from-path>`目录重命名为`<to-path>`

**注意**：移动文件的时候，文件移动到的位置的结尾一定要加上`/` ，不然`mv`命名会把你想要移动的文件重命名成你想移动到的那个位置 

目录名不能和文件同名

#### 复制目录与文件：`cp`
- 复制文件  
`cp <path-to-file> <to-path>`
- 复制目录  
`cp -r <path-to-directory> <to-path>`

#### 查看文本文件内容：`cat`
`cat <fileName>`  

分页查看  
`less <fileName>`  
`j`向下滚屏  
`k`向上滚屏  
`/`字符查找  
`g`按两次，跳到文件头  
`shift-g`跳到文件尾


#### 查看文件类型
```shell
file <fileName>
```


