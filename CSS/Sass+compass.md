### 安装Ruby
#### 采用阿里镜像库

```shell
gem sources --remove https://rubygems.org
gem sources -a https://ruby.taobao.org/
```

#### 安装sass

```shell
gem install sass
gem update (更新ruby)
gem install sass --version=3.3 (安装特定版本sass)
```

#### 编译sass文件

```shell
sass main.scss main.css
```
### 安装compass
#### 安装

```shell
gem install compass
```

#### 创建项目

```shell
compass create learn_compass
```

### 编写sass
#### 两种格式互转

```shell
sass-convert main.scss main.sass
```

### 实战
#### 利用compass新建项目

```shell
compass create learn_compass
```

#### 监听项目文件变化

```shell
compass watch
```

#### 新建变量文件(_variables.scss 局部文件以下划线开头)
#### sass文件引入文件
\- @import "variables";
\- 不同于原生@import

#### 使用css原生@import的既定规则：

- 当@import后边跟的文件名是以.css结尾的时候
- 当@import后边跟的是http://开头的字符串的时候！
- 当@import后边跟的是一个url()函数的时候。
- 当@import后边带有media queries的时候

#### 基于sass的既定规则：
- 没有文件名后缀的时候，sass会添加.scss或者.sass的后缀
- 同一目录下，局部文件和非局部文件不能重名。

#### 注释
\- 类似js, //（不会输出到css文件中） 和 /**/
\- config.rb 文件的output_style=:compressed，即压缩后输出，/\*!..\*/ 在注释开始位置用！可
保留该段注释

#### 变量
\- 以$开头定义变量，类似js

#### 变量操作

- 直接操作变量，即变量表达式
- 局部变量：将变量定义放入属性定义{}内部
- 在选择器、属性名或字符串内使用sass变量或函数，格式为# { $variable / $fn }
- 通过函数
    - 跟代码块无关的函数，多是自己内置函数 ，称为functions
    - 可重用的代码块，称为mixin
        - @include 的方式调用
        - @extend 的方式调用
            - @extend不可继承选择器序列 (例如.cls1 .cls2)

#### @media
sass中的`media query`可以内嵌在css规则中，在生成css的时候`media query`才会被提到样式的最高层级。
**优点**：避免了重复书写选择器或者打乱样式表

#### css输出格式
config.rb配置文件：output_style参数
\- compass compile

### compass配置
#### compass模块
@import "compass" 默认引入除reset和layout之外的5大模块

##### 引入normalize

- gem install compass-normalize
- config.rb 文件头部添加require   'compass-normalize'
- 引入所有模块 @import "normalize";
- 引入部分模块 @import "normalize-version";@import "normalize/base";
- require 'compass/import-once/activate' 该插件解决sass重复引入相同文件的问题，如必须重复引用，@import "compass/reset!" 在文件名后加！

##### layout模块

##### CSS3模块
默认引入browser support模块

##### browser support模块
@debug browsers(); 返回内部浏览器的list

**compass控制台**：
- compass interactive; 进入控制台
- browsers(); 输出浏览器列表
- browser-versions(chrome); 查看具体浏览器版本

**指明支持浏览器**：
- $supported-browsers:chrome,firefox; 多个值间可用,或空格
- $browser-minimum-versions: ("ie":"8"); 指定浏览器的最低版本

##### Typography模块

##### Helpers模块
- compass compile -e production --force;强制制定编译环境为生产环境(development开发环境)
- config.rb文件指定编译环境: #environment = :production or :development

##### Utilities模块
    
    
    






