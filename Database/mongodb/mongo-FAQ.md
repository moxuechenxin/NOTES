## 本地连接远程mongodb数据库
```bash
# mongod.conf

dbpath=/usr/local/mongodb/data
logpath=/usr/local/mongodb/data/mongodb.log
logappend=true
fork=true
bind_ip=0.0.0.0 # 设置后可通过远程连接此服务器
port=27017
```
启动mongodb
```bash
mongod -f <path/to/mongod.conf>
```