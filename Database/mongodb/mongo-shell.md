## 启动mongodb shell
使用`mongo`命令进入启动mongodb shell

```bash
# 显示所有数据库列表
show dbs

# 连接指定数据库
use <db-name>

# 显示当前连接的数据库名称
db
```
数据库名称可以是满足以下条件的任意UTF-8字符串:
- 不能是空字符串（`""`)。
- 不得含有`' '（空格)`、`.`、`$`、`/`、`\`和`\0 (空字符)`
- 应全部小写
- 最多64字节
- 保留的数据库名:
  - `admin`： 从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器。
  - `local`: 这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合
  - `config`: 当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息

## 创建用户并授权
```bash
db.createUser({
  user: 'username',
  pwd: 'password',
  roles: [{
    role: 'userAdminAnyDatabase', # readWrite、read
    db: 'admin'
  }]
})

# 授权
db.auth('username', 'password')
```