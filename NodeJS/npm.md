## npm init
初始化一个简单的`package.json`文件

### npm init执行默认行为
```
npm init --yes
```

### 自定义npm init行为
在目录中创建一个`.npm-init.js`文件

```js
// 使用 prompt() 方法获取用户输入
const desc = prompt('description?', 'A new package...')
const bar = prompt('bar?', '')
const count = prompt('count?', '42')

// module.exports即为package.json配置内容
module.exports = {
  key: 'value',
  foo: {
    bar: bar,
    count: count
  },
  name: prompt('name?', process.cwd().split('/').pop()),
  version: prompt('version?', '0.1.0'),
  description: desc,
  main: 'index.js',
}
```
在该目录执行`npm init`生成以下`package.json`文件内容:
```js
{
  "key": "value",
  "foo": {
    "bar": "",
    "count": "42"
  },
  "name": "hello",
  "version": "0.1.0",
  "description": "A new package...",
  "main": "index.js"
}
```
**注**：除了生成 `package.json`, 因为`.npm-init.js`是一个常规的模块，意味着我们可以执行随便什么node脚本可以执行的任务。例如通过 fs 创建 README, .eslintrc 等项目必需文件，实现项目脚手架的作用

## 依赖包安装
执行`npm install`从`package.json`中的`dependencies`、`devDependencies`将依赖包安装到当前目录的`node_modules`文件夹中

### package定义
```
npm install <package>
```
默认配置下 npm 会从默认的源 (Registry) 中查找该包名对应的包地址，并下载安装。但在 npm 的世界里，除了简单的指定包名, package 还可以是一个指向有效包名的 http url/git url/文件夹路径

以下都是有效的npm package:

\# | 说明 | 例子
---|---|---
a) | 一个包含了程序和描述该程序的 package.json 文件 的 文件夹 | `./local-module/`
b) | 一个包含了 (a) 的 gzip 压缩文件 | `./module.tar.gz`
c) | 一个可以下载得到 (b) 资源的 url (通常是 http(s) url) | `https://registry.npmjs.org/webpack/-/webpack-4.1.0.tgz`
d) | 一个格式为 <name>@<version> 的字符串，可指向 npm 源(通常是官方源 npmjs.org)上已发布的可访问 url，且该 url 满足条件 (c) | `webpack@4.1.0`
e) | 一个格式为 <name>@<tag> 的字符串，在 npm 源上该<tag>指向某 <version> 得到 <name>@<version>，后者满足条件 (d) | `webpack@latest`
f) | 一个格式为 <name> 的字符串，默认添加 latest 标签所得到的 <name>@latest 满足条件 (e) | `webpack`
g) | 一个 git url, 该 url 所指向的代码库满足条件 (a)  [参考](https://docs.npmjs.com/files/package.json#git-urls-as-dependencies) | `git@github.com:webpack/webpack.git` 

### 安装本地包/远程git仓库包
#### 场景1: 本地模块引用
```js
const config = require('../../../../demo.js');
```

**方案**：
1. 创建demo包：
  新增 demo 文件夹; 重命名 demo.js 为 demo/index.js 文件; 创建 package.json 定义 demo 包

```js
{
  "name": "config",
  "main": "index.js",
  "version": "0.1.0"
}
```

2. 在应用层package.json文件中新增依赖项，然后执行`npm install`；或直接执行第3步
```js
{
  "dependencies": {
      "config": "file:./config"
  }
}
```

3. (等价于第2步)直接在应用目录执行`npm install file:./config`

 此时，查看 node_modules 目录我们会发现多出来一个名为 config，指向上层 config/ 文件夹的软链接。npm 识别`file: `协议的url，得知这个包需要直接从文件系统中获取，会自动创建软链接到 node_modules 中，完成“安装”过程

#### 场景2：私有git共享package
可以简单地将被依赖的包托管在私有的 git 仓库中，然后将该 git url 保存到 dependencies 中. npm 会直接调用系统的 git 命令从 git 仓库拉取包的内容到 node_modules 中

npm 支持的 git url 格式：
```
<protocol>://[<user>[:<password>]@]<hostname>[:<port>][:][/]<path>[#<commit-ish> | #semver:<semver>]
```
例如：
```
git+ssh://git@github.com:npm/npm.git#v1.0.27
git+ssh://git@github.com:npm/npm#semver:^5.0
git+https://isaacs@github.com/npm/npm.git
git://github.com/npm/npm.git#v1.0.27
```

#### 场景3：开源package问题修复

**方案**:
fork 原作者的 git 库，在自己所属的 repo 下修复问题后，将 dependencies 中相应的依赖项更改为自己修复后版本的 git url 即可解决问题

## node_modules 目录结构
查看依赖树：
```
npm ls [--depth n]
```

### npm 5 - package-lock 文件
`package-lock.json`的作用是锁定依赖安装结构

**字段说明**：
字段 | 说明
---|---
version | 包的准确版本号
resolved | 安装源
integrity | 内容hash
requires | (最外层为true)包的package.json文件中记录的依赖项
dependencies | node_modules下包目录中的依赖包

禁用package-lock：
```
npm config set package-lock false
```

## 依赖包版本管理
### semver
semver： `MAJOR.MINOR.PATCH`, 意为 `主版本号.小版本号.修订版本号`
- `MAJOR` 对应大的版本号迭代，做了不兼容旧版的修改时要更新 MAJOR 版本号
- `MINOR` 对应小版本迭代，发生兼容旧版API的修改或功能更新时，更新MINOR版本号
- `PATCH` 对应修订版本号，一般针对修复 BUG 的版本号

对于包作者（发布者），npm 要求在publish 之前，必须更新版本号。npm 提供了 npm version 工具，执行 npm version major|minor|patch 可以简单地将版本号中相应的数字加1
> 如果包是一个 git 仓库，npm version 还会自动创建一条注释为更新后版本号的 git commit 和名为该版本号的 tag

在dependencies 中使用 semver 约定的 semver range 指定所需依赖包的版本号或版本范围：
range | 含义 | 例
---|---|---
^2.2.1 | 指定的 MAJOR 版本号下, 所有更新的版本 | 匹配 2.2.3, 2.3.0; 不匹配 1.0.3, 3.0.1
~2.2.1 | 指定 MAJOR.MINOR 版本号下，所有更新的版本 | 匹配 2.2.3, 2.2.9 ; 不匹配 2.3.0, 2.4.5
>=2.1 | 版本号大于或等于 2.1.0 | 匹配 2.1.2, 3.1
<=2.2 | 版本号小于或等于 2.2 | 匹配 1.0.0, 2.2.1, 2.2.11
1.0.0 - 2.0.0 | 版本号从 1.0.0 (含) 到 2.0.0 (含) | 匹配 1.0.0, 1.3.4, 2.0.0

**与，用空格连接任意两条规则**：  
如 `>=2.3.1 <=2.8.0` 可以解读为: `>=2.3.1` 且 `<=2.8.0`
- 可以匹配 `2.3.1`, `2.4.5`, `2.8.0`
- 但不匹配 `1.0.0`, `2.3.0`, `2.8.1`, `3.0.0`

**或，通过`||`连接任意两条规则**：  
如 `^2 >=2.3.1 || ^3 >3.2`
- 可以匹配 2.3.1, 2,8.1, 3.3.1
- 但不匹配 1.0.0, 2.2.0, 3.1.0, 4.0.0

**其他表示方法**：
- `*` 或 `x`匹配所有主版本（`1` 或 `1.x` 匹配 主版本号为 `1` 的所有版本；`1.2` 或 `1.2.x` 匹配 版本号为 `1.2` 开头的所有版本）

**注**：
在常规仅包含数字的版本号之外，semver 还允许在 MAJOR.MINOR.PATCH 后追加 - 后跟点号分隔的标签，作为预发布版本标签，常被视为不稳定、不建议生产使用的版本。例如：
- 1.0.0-alpha
- 1.0.0-beta.1
- 1.0.0-rc.3

`npm install <package name>`默认安装当前最新版本，然后在该版本号前加`^`写入package.json依赖配置。

### 依赖版本升级
> npm >= 5.1
- 升级小版本：本地执行`npm update`
- 升级大版本：本地执行`npm install <package-name>@<version>`

## npm scripts
### 基本使用
通过在`package.json`中`scripts`字段定义一个脚本：
```json
{
    "scripts": {
        "echo": "echo HELLO WORLD"
    }
}
```
就可以通过`npm run echo`命令来执行这段脚本


- `npm run`命令执行时，会把 `./node_modules/.bin/` 目录添加到执行环境的 `PATH` 变量中，因此如果某个命令行包未全局安装，而只安装在了当前项目的 `node_modules` 中，通过 `npm run` 一样可以调用该命令。
- 执行 npm 脚本时要传入参数，需要在命令后加 `--` 标明, 如 `npm run test -- --grep="pattern"` 可以将 `--grep="pattern"` 参数传给 `test` 命令
- npm 提供了 `pre` 和 `post` 两种钩子机制，可以定义某个脚本前后的执行脚本
- 运行时变量：在`npm run`的脚本执行环境内，可以通过环境变量的方式获取许多运行时相关信息，以下都可以通过 `process.env`对象访问获得：
  - `npm_lifecycle_event`：正在运行的脚本名称
  - `npm_package_<key>`：获取当前包 package.json 中某个字段的配置值：如 `npm_package_name` 获取包名
  - `npm_package_<key>_<sub-key>`：package.json 中嵌套字段属性：如 `npm_pacakge_dependencies_webpack` 可以获取到 package.json 中的 dependencies.webpack 字段的值，即 webpack 的版本号

### node_modules/.bin 目录
该目录保存了依赖目录中所安装的可供调用的命令行包

### npx（npm >= 5.2）
```
npx <command>
```

##### 场景1: 一键执行远程 npm 源的二进制包
```
npx cowsay hello
```
npx 将会从 npm 源下载 cowsay 这个包（但并不安装）并执行

##### 场景2: 一键执行 GitHub Gist

##### 场景3: 使用不同版本 node 执行命令

## npm publish
### 发布
1. 注册或登录npm账号

```bash
# 创建账号
npm adduser
# 登陆
npm login

# 使用 npm whoami 查看登陆用户
```

2. 在项目目录中初始化package.json

```bash
npm init [--scope=<your_scope>]
```
推荐添加`README.md`作为包的描述文件

3. 发布

```bash
# 带scope的包默认发布为私人包
# 如果没有 tarball 或 folder 被指定，则使用当前目录
# --tag <tag>：给被发布的包注册指定的 tag，如果没有该参数，则默认使用 latest
npm publish [<tarball>|<folder>] [--tag <tag>] [--access <public|restricted>]
```

### 更新
```bash
# update_type: patch, minor, or major
npm version <update_type>

npm publish
```
该命令会改变package.json中的version，同时会在git仓库中添加一条tag记录

### 取消发布
取消发布一个包，或一个包的某些版本

```bash
npm unpublish [<@scope>/]<pkg>[@<version>]
```

### 弃用
弃用一个包，或一个包的某些版本，尝试安装这些弃用包的用户将会收到警告

```bash
npm deprecate <pkg>[@<version>] <message>
```

## npm配置
### npm config
> 查看 npm 的所有配置，包括默认配置
```
npm config ls -l
```

> 修改配置
```
npm config set <key> <value>
```

> 删除配置
```
npm config delete <key>
```

**常见配置**：
- `proxy`, `https-proxy`： 指定 npm 使用的代理
- `registry`：指定 npm 下载安装包时的源，默认为 `https://registry.npmjs.org/` 可以指定为私有 Registry 源
- `package-lock`：指定是否默认生成 package-lock 文件，建议保持默认 true
- `save`： true/false 指定是否在 npm install 后保存包为 dependencies, npm 5 起默认为 true

### npmrc文件
除了使用 CLI 的 npm config 命令显示更改 npm 配置，还可以通过 npmrc 文件直接修改配置

npmrc 文件优先级由高到低：
- 工程内配置文件: `/path/to/my/project/.npmrc`
- 用户级配置文件: `~/.npmrc`
- 全局配置文件: `$PREFIX/etc/npmrc` (即`npm config get globalconfig`输出的路径)
- npm内置配置文件: `/path/to/npm/npmrc`

### node版本约束
**声明式约束**：通过 package.json 的 engines 属性声明应用运行所需的版本运行时要求。例如我们的项目中使用了 async, await 特性，查阅兼容性表格得知最低支持版本为 7.6.0，因此指定 engines 配置为：
```json
{
  "engines": {
    "node": ">=7.6.0"
  }
}
```

**强约束(可选)**：在 npm 中以上字段内容仅作为建议字段使用，若要在私有项目中添加强约束，需要自己写脚本钩子，读取并解析 engines 字段的 semver range 并与运行时环境做对比校验并适当提醒

## npm最佳实践
- 使用 npm-init 初始化新项目
- 统一项目配置: 需团队共享的 npm config 配置项，固化到 .npmrc 文件中
- 统一运行环境，统一 package.json，统一 package-lock 文件
- 合理使用多样化的源安装依赖包: `npm install <git url>|<local file>`
- 使用 npm: >=5.2 版本
- 使用 npm scripts 与 npx (npm: >=5.2) 脚本管理应用相关脚本

- - -
- [2018 年了，你还是只会 npm install 吗？](https://juejin.im/post/5ab3f77df265da2392364341)