## 本地连接远程mongodb数据库
[参考](https://www.cnblogs.com/gyjerry/p/7291098.html)

> 借助工具robomongo-3T-1.2

```bash
# mongod.conf

dbpath=/usr/local/mongodb/data
logpath=/usr/local/mongodb/data/mongodb.log
logappend=true
fork=true
bind_ip=127.0.0.1
port=27017
```
启动mongodb
```bash
mongod -f <path/to/mongod.conf>
```
