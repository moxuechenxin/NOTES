### npm全局安装的包的存放地址
- **windows**
`C:\Users\Administrator\AppData\Roaming\npm`  
`Administrator`为电脑用户名账号名
- **mac**

### 从镜像源安装
> 推荐使用`nrm`
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

### 锁定安装版本
`npm shrinkwrap`可以用来锁定依赖的版本号

**添加或升级依赖包的方法**:
- 添加或升级一个依赖包
```
npm install --save <pkg>
```

- 升级所有依赖包
```
npm install --no-shrinkwrap
```
注：`npm shrinkwrap`命令加上`--dev`参数才能锁定`devDependencies`中依赖的版本
