## 常用命令
#### 初始化一个新的项目
```bash
yarn init
```

#### 添加一个依赖包
```bash
# --dev : devDependencies
# --peer : peerDependencies
# --optional : optionalDependencies
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```

#### 更新一个依赖包
```bash
yarn upgrade [package]
yarn upgrade [package]@[version]
yarn upgrade [package]@[tag]
```

#### 删除一个依赖包
```bash
yarn remove [package]
```

#### 安装所有的依赖包
```bash
# --flat : Installing one and only one version of a package
# --force : Forcing a re-download of all packages
# --production : Installing only production dependencies
yarn [install]
```