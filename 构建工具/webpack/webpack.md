# webpack 的概念和基础使用
## 安装
一般把 webpack 作为项目的开发依赖来安装使用
```
npm install webpack webpack-cli -D

# 或者
yarn add webpack webpack-cli -D
```

## 基本概念
#### entry
```js
module.exports = {
  entry: './src/index.js' 
}

// 上述配置等同于
module.exports = {
  entry: {
    main: './src/index.js'
  }
}

// 或者配置多个入口
module.exports = {
  entry: {
    foo: './src/page-foo.js',
    bar: './src/page-bar.js', 
    // ...
  }
}

// 使用数组来对多个文件进行打包
module.exports = {
  entry: {
    main: [
      './src/foo.js',
      './src/bar.js'
    ]
  }
}
```

#### loader
负责把某种文件格式的内容转换成 webpack 可以支持打包的模块

```js
/*
 *  使用 Babel 来处理 .js 文件
 */
 
module: {
  // ...
  rules: [
    {
      test: /\.jsx?/, // 匹配文件路径的正则表达式，通常我们都是匹配文件类型后缀
      include: [
        path.resolve(__dirname, 'src') // 指定哪些路径下的文件需要经过 loader 处理
      ],
      use: 'babel-loader', // 指定使用的 loader
    },
  ],
}
```

#### plugin
模块代码转换的工作由 loader 来处理，除此之外的其他任何工作都可以交由 plugin 来完成

```js
const UglifyPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
  plugins: [
    new UglifyPlugin()
  ],
}
```

#### 输出(output)
```js
module.exports = {
  // ...
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },
}

// 或者多个入口生成不同文件
module.exports = {
  entry: {
    foo: './src/foo.js',
    bar: './src/bar.js',
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist',
  },
}

// 路径中使用 hash，每次构建时会有一个不同 hash 值，避免发布新版本时线上使用浏览器缓存
module.exports = {
  // ...
  output: {
    filename: '[name].js',
    path: __dirname + '/dist/[hash]',
  },
}
```
默认创建的输出内容就是`./dist/main.js`

#### 一个简单的 webpack 配置
> webpack.config.js

```js
const path = require('path')
const UglifyPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
  entry: './src/index.js',

  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
  },

  module: {
    rules: [
      {
        test: /\.jsx?/,
        include: [
          path.resolve(__dirname, 'src')
        ],
        use: 'babel-loader',
      },
    ],
  },

  // 代码模块路径解析的配置
  resolve: {
    modules: [
      "node_modules",
      path.resolve(__dirname, 'src')
    ],

    extensions: [".wasm", ".mjs", ".js", ".json", ".jsx"],
  },

  plugins: [
    new UglifyPlugin(), 
    // 使用 uglifyjs-webpack-plugin 来压缩 JS 代码
    // 如果你留意了我们一开始直接使用 webpack 构建的结果，你会发现默认已经使用了 JS 代码压缩的插件
    // 这其实也是我们命令中的 --mode production 的效果，后续的小节会介绍 webpack 的 mode 参数
  ],
}
```

# 搭建基本的前端开发环境
## 关联HTML
使用[`html-webpack-plugin`](https://github.com/jantimon/html-webpack-plugin)

> webpack配置

```js
module.exports = {
  // ...
  plugins: [
    new HtmlWebpackPlugin({
      filename: 'index.html', // 配置输出文件名和路径
      template: 'assets/index.html', // 配置文件模板
    }),
  ],
}
```

## 构建 CSS
```
module.exports = {
  module: {
    rules: [
      // ...
      {
        test: /\.css/,
        include: [
          path.resolve(__dirname, 'src'),
        ],
        use: [
          'style-loader',
          'css-loader',
        ],
      },
    ],
  }
}
```
在index.js中引用index.css
```js
import "./index.css"
```

- `css-loader`：负责解析 CSS 代码，主要是为了处理 CSS 中的依赖，例如 `@import` 和 `url()` 等引用外部文件的声明；

- `style-loader`：会将 css-loader 解析的结果转变成 JS 代码，运行时动态插入 `style` 标签来让 CSS 代码生效

经由上述两个 loader 的处理后，CSS 代码会转变为 JS，和 index.js 一起打包了。如果需要单独把 CSS 文件分离出来，我们需要使用 `extract-text-webpack-plugin`插件

```js
const ExtractTextPlugin = require('extract-text-webpack-plugin')

module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.css$/,
        // 因为这个插件需要干涉模块转换的内容，所以需要使用它对应的 loader
        use: ExtractTextPlugin.extract({ 
          fallback: 'style-loader',
          use: 'css-loader',
        }), 
      },
    ],
  },
  plugins: [
    // 引入插件，配置文件名，这里同样可以使用 [hash]
    new ExtractTextPlugin('index.css'),
  ],
}
```

## CSS预处理器
> 处理less文件

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.less$/,
        // 因为这个插件需要干涉模块转换的内容，所以需要使用它对应的 loader
        use: ExtractTextPlugin.extract({ 
          fallback: 'style-loader',
          use: [
            'css-loader', 
            'less-loader',
          ],
        }), 
      },
    ],
  },
  // ...
}
```

## 处理图片文件
> file-loader

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'file-loader',
            options: {},
          },
        ],
      },
    ],
  },
}
```

## 使用Babel

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.jsx?/, // 支持 js 和 jsx
        include: [
          path.resolve(__dirname, 'src'), // src 目录下的才需要经过 babel-loader 处理
        ],
        loader: 'babel-loader',
      },
    ],
  },
}
```
Babel 的相关配置可以在目录下使用 .babelrc 文件来处理，详细参考 Babel 官方文档[`.babelrc`](http://babeljs.io/docs/usage/babelrc/)

## 启动静态服务
在项目下安装 webpack-dev-server，然后添加启动命令到 package.json 中：

```json
"scripts": {
  "build": "webpack --mode production",
  "start": "webpack-dev-server --mode development"
}
```

# weboack如何解析代码模块路径

## 模块解析规则
#### 解析模块名
查找当前文件目录下，父级目录及以上目录下的 `node_modules` 文件夹，看是否有对应名称的模块

#### 解析相对路径
1. 查找相对当前模块的路径下是否有对应文件或文件夹
2. 是文件则直接加载
3. 是文件夹则继续查找文件夹下的 package.json 文件
4. 有 package.json 文件则按照文件中 `main` 字段的文件名来查找文件
5. 无 package.json 或者无 `main` 字段则查找 `index.js` 文件

#### 解析绝对路径(不建议使用)
直接查找对应路径的文件


#### webpack的resolve
在 webpack 配置中，和模块路径解析相关的配置都在 resolve 字段下
```js
module.exports = {
  resolve: {
    // 路径别名
    alias: {
        utils: path.resolve(__dirname, 'src/utils'),
        utils$: path.resolve(__dirname, 'src/utils') // 只会匹配 import 'utils'
    },
    // 补全后缀名（优先级从左到右）
    extensions: ['.wasm', '.mjs', '.js', '.json', '.jsx'],
    // 模块查找路径
    modules: [
        path.resolve(__dirname, 'node_modules'), // 指定当前目录下的 node_modules 优先查找
        'node_modules', // 如果有一些类库是放在一些奇怪的地方的，你可以添加自定义的路径或者目录
    ],
    // mainFields决定package.json 中使用哪个字段导入模块
    mainFields: {
        /* 默认值如下： */
        // 配置 target === "web" 或者 target === "webworker" 时 mainFields 默认值是：
        mainFields: ['browser', 'module', 'main'],
        // target 的值为其他时，mainFields 默认值为：
        mainFields: ["module", "main"]
    }，
    // 当目录下没有 package.json 文件时，要查找的文件（默认为index.js）
    mainFiles: [index]
  }
}
```

# 配置loader
## loader匹配规则
```js
module.exports = {
  // ...
  module: {
    rules: [ 
      {
        test: /\.jsx?/, // 条件
        include: [ 
          path.resolve(__dirname, 'src'),
        ], // 条件
        use: 'babel-loader', // 规则应用结果
      }, // 一个 object 即一条规则
      // ...
    ],
  },
}
```
`test`、 `include`是`resource.test`和`resource.include`的简写

## 规则条件配置
**配置形式**：
- `{ test: ... }` 匹配特定条件
- `{ include: ... }` 匹配特定路径
- `{ exclude: ... }` 排除特定路径
- `{ and: [...] }`必须匹配数组中所有条件
- `{ or: [...] }` 匹配数组中任意一个条件
- `{ not: [...] ` 排除匹配数组中所有条件

上述的所谓条件的值可以是：
- **字符串**：必须以提供的字符串开始，所以是字符串的话，这里我们需要提供绝对路径
- **正则表达式**：调用正则的 test 方法来判断匹配
- **函数**：(path) => boolean，返回 true 表示匹配
- **数组**：至少包含一个条件的数组
- **对象**：匹配所有属性值的条件

```js
rules: [
  {
    test: /\.jsx?/, // 正则
    include: [
      path.resolve(__dirname, 'src'), // 字符串，注意是绝对路径
    ], // 数组
    // ...
  },
  {
    test: {
      js: /\.js/,
      jsx: /\.jsx/,
    }, // 对象，不建议使用
    not: [
      (value) => { /* ... */ return true; }, // 函数，通常需要高度自定义时才会使用
    ],
  },
]
```

## module type
**模块类型**：
- `javascript/auto`：即 webpack 3 默认的类型，支持现有的各种 JS 代码模块类型 —— CommonJS、AMD、ESM
- `javascript/esm`：ECMAScript modules，其他模块系统，例如 CommonJS 或者 AMD 等不支持，是 .mjs 文件的默认类型
- `javascript/dynamic`：CommonJS 和 AMD，排除 ESM
- `javascript/json`：JSON 格式数据，require 或者 import 都可以引入，是 .json 文件的默认类型
- `webassembly/experimental`：WebAssembly modules，当前还处于试验阶段，是 .wasm 文件的默认类型

如果不希望使用默认的类型的话，在确定好匹配规则条件时，我们可以使用 type 字段来指定模块类型
```js
/* 把所有的 JS 代码文件都设置为强制使用 ESM 类型 */
{
  test: /\.js/,
  include: [
    path.resolve(__dirname, 'src'),
  ],
  type: 'javascript/esm', // 这里指定模块类型
},
```

## 使用loader配置
```js
rules: [
  {
    test: /\.less/,
    use: [
      'style-loader', // 直接使用字符串表示 loader
      {
        loader: 'css-loader',
        options: {
          importLoaders: 1
        },
      }, // 用对象表示 loader，可以传递 loader 配置等
      {
        loader: 'less-loader',
        options: {
          noIeCompat: true
        }, // 传递 loader 配置
      },
    ],
  },
],
```
`use` 字段可选的类型：
- 一个字符串
- 一个数组
- 一个表示 loader 的对象

## loader应用顺序
#### 一个匹配规则中使用的多个loader
从最后配置的loader开始，一步步往前应用loader

#### 多个rule匹配同一个模块文件
例如：
```js
// 无法保证 eslint-loader 在 babel-loader 应用前执行
rules: [
  {
    test: /\.js$/,
    exclude: /node_modules/,
    loader: "eslint-loader",
  },
  {
    test: /\.js$/,
    exclude: /node_modules/,
    loader: "babel-loader",
  },
]
```
webpack 在 rules 中提供了一个 `enforce` 的字段来配置当前 rule 的 loader 类型，没配置的话是普通类型，我们可以配置 `pre` 或 `post`，分别对应前置类型或后置类型的 loader

还有一种行内 loader，即我们在应用代码中引用依赖时直接声明使用的 loader，如 `const json = require('json-loader!./file.json')`这种（不建议在应用开发中使用这种 loader）

所有的 loader 按照**前置** -> **行内** -> **普通** -> **后置**的顺序执行

```js
// 设置为：eslint-loader 在 babel-loader 之前执行
rules: [
  {
    enforce: 'pre', // 指定为前置类型
    test: /\.js$/,
    exclude: /node_modules/,
    loader: "eslint-loader",
  },
]
```

## 使用`noParse`
`module.noParse` 字段，可以用于配置哪些模块文件的内容不需要进行解析

> 使用 `noParse` 进行忽略的模块文件中不能使用 `import`、`require`、`define` 等导入机制

```js
module.exports = {
  // ...
  module: {
    noParse: /jquery|lodash/, // 正则表达式

    // 或者使用 function
    noParse(content) {
      return /jquery|lodash/.test(content)
    },
  }
}
```

# 使用plugin
## DefinePlugin
webpack内置的插件，可以使用 `webpack.DefinePlugin` 直接获取，用于创建一些在编译时可以配置的全局常量

```js
module.exports = {
  // ...
  plugins: [
    new webpack.DefinePlugin({
      PRODUCTION: JSON.stringify(true), // const PRODUCTION = true
      VERSION: JSON.stringify('5fa3b9'), // const VERSION = '5fa3b9'
      BROWSER_SUPPORTS_HTML5: true, // const BROWSER_SUPPORTS_HTML5 = 'true'
      TWO: '1+1', // const TWO = 1 + 1,
      CONSTANTS: {
        APP_VERSION: JSON.stringify('1.1.2') // const CONSTANTS = { APP_VERSION: '1.1.2' }
      }
    }),
  ],
}
```

配置规则：
- 如果配置的值是字符串，那么整个字符串会被当成代码片段来执行，其结果作为最终变量的值，如上面的 "1+1"，最后的结果是 2
- 如果配置的值不是字符串，也不是一个对象字面量，那么该值会被转为一个字符串，如 true，最后的结果是 'true'
- 如果配置的是一个对象字面量，那么该对象的所有 key 会以同样的方式去定义

## copy-webpack-plugin
```js
const CopyWebpackPlugin = require('copy-webpack-plugin')

module.exports = {
  // ...
  plugins: [
    new CopyWebpackPlugin([
      { from: 'src/file.txt', to: 'build/file.txt', }, // 顾名思义，from 配置来源，to 配置目标路径
      { from: 'src/*.ico', to: 'build/*.ico' }, // 配置项可以使用 glob
      // 可以配置很多项复制规则
    ]),
  ],
}
```

## extract-text-webpack-plugin
把依赖的 CSS 分离出来成为单独的文件
```js
const ExtractTextPlugin = require('extract-text-webpack-plugin')

module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.css$/,
        // 因为这个插件需要干涉模块转换的内容，所以需要使用它对应的 loader
        use: ExtractTextPlugin.extract({ 
          fallback: 'style-loader',
          use: 'css-loader',
        }), 
      },
    ],
  },
  plugins: [
    // 引入插件，配置文件名，这里同样可以使用 [hash]
    new ExtractTextPlugin('[name].css'),
  ],
}
```
**注**：除了在 plugins 字段添加插件实例之外，还需要调整 loader 对应的配置

## ProvidePlugin
webpack 内置的插件，可以直接使用 `webpack.ProvidePlugin` 来获取，该插件用于引用某些模块作为应用运行时的变量，从而不必每次都用 `require` 或者 `import`

```js
new webpack.ProvidePlugin({
  identifier: 'module',
  // ...
})

// 或者
new webpack.ProvidePlugin({
  identifier: ['module', 'property'], // 即引用 module 下的 property，类似 import { property } from 'module'
  // ...
}).
```
**注意**：如果是 ES 的 `default export`，那么你需要指定模块的 default 属性：`identifier: ['module', 'default']`

## IgnorePlugin
webpack 内置的插件，可以直接使用 `webpack.IgnorePlugin` 来获取，这个插件用于忽略某些特定的模块，让 webpack 不把这些指定的模块打包进去

```js
/* 忽略moment.js中的i18n代码 */
module.exports = {
  // ...
  plugins: [
    new webpack.IgnorePlugin(/^\.\/locale$/, /moment$/)
  ]
}
```
`IgnorePlugin` 配置的参数有两个，第一个是匹配引入模块路径的正则表达式，第二个是匹配模块的对应上下文，即所在目录名

# 更好地使用 webpack-dev-server
## webpack-dev-server 的配置
在 webpack 的配置中，可以通过 `devServer` 字段来配置 webpack-dev-server，如端口设置、启动 gzip 压缩等：
- `public`：用于指定静态服务的域名，默认是 http://localhost:8080/ ，当你使用 Nginx 来做反向代理时，应该就需要使用该配置来指定 Nginx 配置使用的服务域名

- `port`： 字段用于指定静态服务的端口，如上，默认是 8080，通常情况下都不需要改动

- `publicPath`：用于指定构建好的静态文件在浏览器中用什么路径去访问，默认是 `/`（如果使用了 HMR，那么要设置 `publicPath` 就必须使用完整的 URL，建议将 `devServer.publicPath` 和 `output.publicPath` 的值保持一致）

- `proxy`：用于配置 webpack-dev-server 将特定 URL 的请求代理到另外一台服务器上
```js
proxy: {
  '/api': {
    target: "http://api.example.com", // 将 URL 中带有 /api 的请求代理到http://api.example.com的服务上
    pathRewrite: { '^/api': '' }, // 把 URL 中 path 部分的 `api` 移除掉
  },
}
```

- `contentBase`：用于配置提供额外静态文件内容的目录，之前提到的 publicPath 是配置构建好的结果以什么样的路径去访问，而 contentBase 是配置额外的静态文件内容的访问路径，即那些不经过 webpack 构建，但是需要在 webpack-dev-server 中提供访问的静态资源（如部分图片等）

> `publicPath` 的优先级高于 `contentBase`

```js
// 使用当前目录下的 public
contentBase: path.join(__dirname, "public") 

// 也可以使用数组提供多个路径
contentBase: [path.join(__dirname, "public"), path.join(__dirname, "assets")]
```

- `before`： 在 webpack-dev-server 静态资源中间件处理之前，可以用于拦截部分请求返回特定内容，或者实现简单的数据 mock

- `after`： 在 webpack-dev-server 静态资源中间件处理之后，比较少用到，可以用于打印日志或者做一些额外处理

```js
before(app){
  app.get('/some/path', function(req, res) { // 当访问 /some/path 路径时，返回自定义的 json 数据
    res.json({ custom: 'response' })
  })
}
```

## webpack-dev-middleware
webpack-dev-middleware是在 Express 中提供 webpack-dev-server 静态服务能力的一个中间件

```js
/* 一个 Node.js 服务的脚本文件: app.js */

const webpack = require('webpack')
const middleware = require('webpack-dev-middleware')
const webpackOptions = require('./webpack.config.js') // webpack 配置文件的路径

// 本地的开发环境默认就是使用 development mode
webpackOptions.mode = 'development'

const compiler = webpack(webpackOptions)
const express = require('express')
const app = express()

app.use(middleware(compiler, {
  // webpack-dev-middleware 的配置选项
}))

// 其他 Web 服务中间件
// app.use(...)

app.listen(3000, () => console.log('Example app listening on port 3000!'))
```

# 开发和生产环境的构建配置差异
webpack 4.x 版本引入了 `mode` 的概念，在运行 webpack 时需要指定使用 production 或 development 两个 mode 其中一个，这个功能也就是我们所需要的运行两套构建环境的能力

当指定使用 `production` mode 时，默认会启用各种性能优化的功能，包括构建结果优化以及 webpack 运行性能优化，而如果是 `development` mode 的话，则会开启 debug 工具，运行时打印详细的错误信息，以及更加快速的增量编译构建

## 在配置文件中区分 mode
配置文件可以对外暴露一个函数
```js
module.exports = (env, argv) => ({
  // ... 其他配置
  optimization: {
    minimize: false,
    // 使用 argv 来获取 mode 参数的值
    minimizer: argv.mode === 'production' ? [
      new UglifyJsPlugin({ /* 你自己的配置 */ }), 
      // 仅在我们要自定义压缩配置时才需要这么做
      // mode 为 production 时 webpack 会默认使用压缩 JS 的 plugin
    ] : [],
  },
})
```

## 运行时的环境变量
我们使用 webpack 时传递的 mode 参数，是可以在我们的应用代码运行时，通过 `process.env.NODE_ENV` 这个变量获取的

## 常见的环境差异配置
- 生产环境可能需要分离 CSS 成单独的文件，以便多个页面共享同一个 CSS 文件
- 生产环境需要压缩 HTML/CSS/JS 代码
- 生产环境需要压缩图片
- 开发环境需要生成 sourcemap 文件
- 开发环境需要打印 debug 信息
- 开发环境需要 live reload 或者 hot reload 的功能

## 拆分配置
可以把 webpack 的配置按照不同的环境拆分成多个文件，运行时直接根据环境变量加载对应的配置即可。基本的划分如下：

- `webpack.base.js`：基础部分，即多个文件中共享的配置
- `webpack.development.js`：开发环境使用的配置
- `webpack.production.js`：生产环境使用的配置
- `webpack.test.js`：测试环境使用的配置

使用`webpack-merge`合并多个配置对象：

```js
/* webpack.base.js */

module.exports = {
  entry: '...',
  output: {
    // ...
  },
  resolve: {
    // ...
  },
  module: {
    // 这里是一个简单的例子，后面介绍 API 时会用到
    rules: [
      {
        test: /\.js$/, 
        use: ['babel'],
      },
    ],
    // ...
  },
  plugins: [
    // ...
  ],
}
```

```js
/* webpack.development.js */

const { smart } = require('webpack-merge')
const webpack = require('webpack')
const base = require('./webpack.base.js')

module.exports = smart(base, {
  module: {
    rules: [
      // 用 smart API，当这里的匹配规则相同且 use 值都是数组时，smart 会识别后处理
      // 和上述 base 配置合并后，这里会是 { test: /\.js$/, use: ['babel', 'coffee'] }
      // 如果这里 use 的值用的是字符串或者对象的话，那么会替换掉原本的规则 use 的值
      {
        test: /\.js$/,
        use: ['coffee'],
      },
      // ...
    ],
  },
  plugins: [
    // plugins 这里的数组会和 base 中的 plugins 数组进行合并
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV),
    }),
  ],
})
```

# 用HMR提高开发效率
HMR 全称是 Hot Module Replacement，即模块热替换

## 配置使用 HMR
```js
const webpack = require('webpack')

module.exports = {
  // ...
  devServer: {
    hot: true // dev server 的配置要启动 hot，或者在命令行中带参数开启
  },
  plugins: [
    // ...
    new webpack.NamedModulesPlugin(), // 用于启动 HMR 时可以显示模块的相对路径
    new webpack.HotModuleReplacementPlugin(), // Hot Module Replacement 的插件
  ],
}
```

## HMR 运行原理

# 优化前端资源加载 1 - 图片加载优化和代码压缩
## CSS Sprites
## 图片压缩
如下是利用`image-webpack-loader`来压缩图片的配置
```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /.*\.(gif|png|jpe?g|svg|webp)$/i,
        use: [
          {
            loader: 'file-loader',
            options: {}
          },
          {
            loader: 'image-webpack-loader',
            options: {
              mozjpeg: { // 压缩 jpeg 的配置
                progressive: true,
                quality: 65
              },
              optipng: { // 使用 imagemin-optipng 压缩 png，enable: false 为关闭
                enabled: false,
              },
              pngquant: { // 使用 imagemin-pngquant 压缩 png
                quality: '65-90',
                speed: 4
              },
              gifsicle: { // 压缩 gif 的配置
                interlaced: false,
              },
              webp: { // 开启 webp，会把 jpg 和 png 图片压缩为 webp 格式
                quality: 75
              },
          },
        ],
      },
    ],
  },
}
```

## 使用DataURL
url-loader 和 file-loader 的功能类似，但是在处理文件的时候，可以通过配置指定一个大小，当文件小于这个配置值时，url-loader 会将其转换为一个 base64 编码的 DataURL

```js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192, // 单位是 Byte，当文件小于 8KB 时作为 DataURL 处理
            },
          },
        ],
      },
    ],
  },
}
```

## 代码压缩
webpack 4.x 版本运行时，mode 为 production 即会启动压缩 JS 代码的插件

> 使用html-webpack-plugin 插件对HTML进行压缩：
```js
module.exports = {
  // ...
  plugins: [
    new HtmlWebpackPlugin({
      filename: 'index.html', // 配置输出文件名和路径
      template: 'assets/index.html', // 配置文件模板
      minify: { // 压缩 HTML 的配置
        minifyCSS: true, // 压缩 HTML 中出现的 CSS 代码
        minifyJS: true // 压缩 HTML 中出现的 JS 代码
      }
    }),
  ],
}
```

> 使用css-loader压缩css：
```js
module.exports = {
  module: {
    rules: [
      // ...
      {
        test: /\.css/,
        include: [
          path.resolve(__dirname, 'src'),
        ],
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              minimize: true, // 使用 css 的压缩功能
            },
          },
        ],
      },
    ],
  }
}
```

# 优化前端资源加载 2 - 分离代码文件
## 分离代码文件
## webpack 4.x 的 optimization
```js
module.exports = {
  // ... webpack 配置

  optimization: {
    splitChunks: {
      chunks: "all", // 所有的 chunks 代码公共的部分分离出来成为一个单独的文件
    },
  },
}
```

> 显式配置共享类库
```js
module.exports = {
  entry: {
    vendor: ["react", "lodash", "angular", ...], // 指定公共使用的第三方类库
  },
  optimization: {
    splitChunks: {
      cacheGroups: {
        vendor: {
          chunks: "initial",
          test: "vendor",
          name: "vendor", // 使用 vendor 入口作为公共部分
          enforce: true,
        },
      },
    },
  },
  // ... 其他配置
}

// 或者
module.exports = {
  optimization: {
    splitChunks: {
      cacheGroups: {
        vendor: {
          test: /react|angluar|lodash/, // 直接使用 test 来做路径匹配
          chunks: "initial",
          name: "vendor",
          enforce: true,
        },
      },
    },
  },
}

// 或者
module.exports = {
  optimization: {
    splitChunks: {
      cacheGroups: {
        vendor: {
          chunks: "initial",
          test: path.resolve(__dirname, "node_modules") // 路径在 node_modules 目录下的都作为公共部分
          name: "vendor", // 使用 vendor 入口作为公共部分
          enforce: true,
        },
      },
    },
  },
}
```

# 优化前端资源加载 3 - 进一步控制 JS 大小
## 按需加载模块
> 使用import()

```js
// import 作为一个方法使用，传入模块名即可，返回一个 promise 来获取模块暴露的对象
// 注释 webpackChunkName: "lodash" 可以用于指定 chunk 的名称，在输出文件时有用
import(/* webpackChunkName: "lodash" */ 'lodash').then((_) => { 
  console.log(_.lash([1, 2, 3])) // 打印 3
})
```

**注意**：
1. 如果使用了Babel，需配置插件[`babel-plugin-syntax-dynamic-import`](https://babeljs.io/docs/plugins/syntax-dynamic-import/)来处理 `import()` 语法
2. 由于动态加载代码模块的语法依赖于 promise，对于低版本的浏览器，需要添加 promise 的 [polyfill](https://github.com/stefanpenner/es6-promise) 后才能使用

import 后面的注释 `webpackChunkName: "lodash"` 用于告知 webpack 所要动态加载模块的名称（即指定打包出来的name），在 webpack 配置中添加一个 `output.chunkFilename` 的配置
```js
output: {
  path: path.resolve(__dirname, 'dist'),
  filename: '[name].[hash:8].js',
  chunkFilename: '[name].[hash:8].js' // 指定分离出来的代码文件的名称
}
```

## Tree shaking
Tree shaking 依赖于 ES2015 模块系统中的静态结构特性，可以移除 JavaScript 上下文中的未引用代码，删掉用不着的代码，能够有效减少 JS 代码文件的大小
```js
// src/math.js
export function square(x) {
  return x * x;
}

export function cube(x) {
  return x * x * x;
}

// src/index.js
import { cube } from './math.js' // 在这里只是引用了 cube 这个方法

console.log(cube(3))
```

**注意**：
1. 在 webpack 中，只有启动了 JS 代码压缩功能（即使用 uglify）时，会做 Tree shaking 的优化。webpack 4.x 需要指定 mode 为 production，而 webpack 3.x 的话需要配置 UglifyJsPlugin

2. 如果在项目中使用了 Babel 的话，要把 Babel 解析模块语法的功能关掉，在 .babelrc 配置中增加 "modules": false 这个配置
```json
{
  "presets": [["env", { "modules": false }]]
}
```

## sideEffects

# 提升 webpack 的构建速度
## 让 webpack 少干点活
#### 减少 resolve 的解析
```js
resolve: {
  modules: [
    path.resolve(__dirname, 'node_modules'), // 使用绝对路径指定 node_modules，不做过多查询
  ],

  // 删除不必要的后缀自动补全，少了文件后缀的自动匹配，即减少了文件路径查询的工作
  // 其他文件可以在编码时指定后缀，如 import('./index.scss')
  extensions: [".js"], 

  // 避免新增默认文件，编码时使用详细的文件路径，代码会更容易解读，也有益于提高构建速度
  mainFiles: ['index'],
},
```

#### 把 loader 应用的文件范围缩小
```js
rules: [ 
  {
    test: /\.jsx?/,
    include: [ 
      path.resolve(__dirname, 'src'), 
      // 限定只在 src 目录下的 js/jsx 文件需要经 babel-loader 处理
      // 通常我们需要 loader 处理的文件都是存放在 src 目录
    ],
    use: 'babel-loader',
  },
  // ...
],
```

#### 减少 plugin 的消耗

#### 换种方式处理图片

#### 使用 DLLPlugin
1. 额外的配置文件webpack.dll.config.js
