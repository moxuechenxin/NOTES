### npm全局安装的包的存放地址
- **windows**
`C:\Users\Administrator\AppData\Roaming\npm`  
`Administrator`为电脑用户名账号名
- **mac**

### 从镜像源安装
- **执行安装命令时指定**  
添加`--registry=http://registry.url`


```shell
# 使用淘宝镜像源
npm install underscore --registry=https://registry.npm.taobao.org
```
- **全局指定**

```shell
npm config set registry https://registry.npm.taobao.org
```

### require()

