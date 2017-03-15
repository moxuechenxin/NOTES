## 安装和使用
### 1. 全局安装gulp：

```shell
npm install -g gulp
```

### 2. 新建package.json文件

```shell
npm init
```

### 3. 作为项目的开发依赖（devDependencies）安装：
全局安装gulp是为了执行gulp任务，本地安装gulp则是为了调用gulp插件的功能

```shell
npm install --save-dev gulp
```

### 4. 在项目根目录下创建一个名为`gulpfile.js`的文件：

```js
var gulp = require('gulp');

// 创建任务
gulp.task('default',function(){
    // 默认认为
});
```

### 5. 运行gulp：

```
gulp [<task1>...<taskN>]
```

## API
### gulp.src(globs [, options])
输出（Emits）符合所提供的匹配模式（glob）或者匹配模式的数组（array of globs）的文件。 将返回一个 Vinyl files 的 stream 它可以被 piped 到别的插件中。

```js
gulp.src('client/templates/*.jade')
  .pipe(jade())
  .pipe(minify())
  .pipe(gulp.dest('build/minified_templates'));
```

**参数**:
- **globs**：(String/Array)   
    所要读取的 `glob` 或者包含 `globs` 的数组  

    **通配符路径匹配示例**：
    - `src/a.js`：指定具体文件；
    
    - `*`：匹配文件路径中的0个或多个字符，但不会匹配路径分隔符，除非路径分隔符出现在末尾    
     **例**：`*` 能匹配 `a.js`，`x.y`，`abc`，`abc/`，但不能匹配`a/b.js`
    
    - `**`：匹配路径中的0个或多个目录及其子目录,需要单独出现，即它左右不能有其他东西了。如果出现在末尾，也能匹配文件   
    **例**：  
    `src/**/*.js`(包含src的0个或多个子文件夹下的js文件)；  
    `**` 能匹配 `abc`，`a/b.js`，`a/b/c.js`，`x/y/z`，`x/y/z/a.b`，能用来匹配所有的目录和文件
    
    - `?`：匹配文件路径中的一个字符(不会匹配路径分隔符)  
    **例**：  
    `?.js` 能匹配 `a.js`,`b.js`,`c.js`;  
    `a??` 能匹配 `a.b`,`abc`,但不能匹配`ab/`,因为它不会匹配路径分隔符
    
    - `[...]`：类似正则的`[...]`，其中`!`效果同`^`  
    **例**：  
    `[xyz].js` 只能匹配`x.js`，`y.js`，`z.js`，不会匹配`xy.js`，`xyz.js`等,整个中括号只代表一个字符  
    `[^xyz].js` 能匹配 `a.js`，`b.js`，`c.js`等，不能匹配`x.js`，`y.js`，`z.js`
    
    - `!(pattern|pattern|pattern)`：匹配任何与括号中给定的任一模式都不匹配的  
    **例**：`!src/a.js` 不包含src下的a.js文件；
    
    - `?(pattern|pattern|pattern)`：类似正则中的`(pattern|pattern|pattern)?`
    
    - `+(pattern|pattern|pattern)`：类似正则中的`(pattern|pattern|pattern)+`
    
    - `*(pattern|pattern|pattern)`：类似正则中的`(pattern|pattern|pattern)*`
    
    - `@(pattern|pattern|pattern)`：类似正则中的`(pattern|pattern|pattern)`
    
    - `{}`：匹配多个属性  
    **例**：  
    `src/{a,b}.js` 包含`a.js`和`b.js`文件;   
    `src/*.{jpg,png,gif}` src下的所有`jpg/png/gif`文件；
    

- **options**：(Object)  
    通过 `glob-stream` 所传递给 `node-glob` 的参数。  
    除了 `node-glob` 和 `glob-stream` 所支持的参数外，gulp 增加了一些额外的选项参数：
    - **options.buffer**：(Boolean ; 默认值：true)  
    如果该项被设置为 false，那么将会以 stream 方式返回 file.contents 而不是文件 buffer 的形式。这在处理一些大文件的时候将会很有用。  
    **注意**：插件可能并不会实现对 stream 的支持。

    - **options.read**：(Boolean ; 默认值：true)  
    如果该项被设置为 false， 那么 file.contents 会返回空值（null），也就是并不会去读取文件。
    
    - **options.base**：(String ; 默认值：)
    
```js
/*
    假设路径 client/js/somdir 的目录中，有一个文件叫 somefile.js
*/
gulp.src('client/js/**/*.js') // 匹配 'client/js/somedir/somefile.js' 并且将 `base` 解析为 `client/js/`
  .pipe(minify())
  .pipe(gulp.dest('build'));  // 写入 'build/somedir/somefile.js'

gulp.src('client/js/**/*.js', { base: 'client' })
  .pipe(minify())
  .pipe(gulp.dest('build'));  // 写入 'build/js/somedir/somefile.js'

```

### gulp.dest( path [, options] )
能被 pipe 进来，并且将会写文件。并且重新输出（emits）所有数据，因此你可以将它 pipe 到多个文件夹。如果某文件夹不存在，将会自动创建它。

```js
gulp.src('./client/templates/*.jade')
  .pipe(jade())
  .pipe(gulp.dest('./build/templates'))
  .pipe(minify())
  .pipe(gulp.dest('./build/minified_templates'));
```
文件被写入的路径是以所给的相对路径根据所给的目标目录计算而来。类似的，相对路径也可以根据所给的 base 来计算。

**参数**：
- **path**：(String/Function)  
文件将被写入的路径（输出目录）。也可以传入一个函数，在函数中返回相应路径，这个函数也可以由 `vinyl 文件实例`来提供
- **options**：(Object)
    - **options.cwd**：(String ; 默认值：`process.cwd()`)  
    输出目录的 `cwd` 参数，只在所给的输出目录是相对路径时候有效。
    - **options.mode**：(String ; 默认值：`0777`)  
    八进制权限字符，用以定义所有在输出目录中所创建的目录的权限。

### gulp.task( name [, deps] , fn)
定义一个使用 Orchestrator 实现的任务（task）

```js
gulp.task('somename', function() {
  // 做一些事
});
```
**参数**：
- **name**：(String)  
  任务的名字（如果你需要在命令行中运行你的某些任务，那么，请不要在名字中使用空格）。

- **deps**：(Array)  
  一个包含任务列表的数组，这些任务会在你当前任务运行之前完成（这些任务将以最大的并发数执行）

```js
gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
  // 做一些事
});
```
**注意**：你的任务是否在这些前置依赖的任务完成之前运行了？请一定要确保你所依赖的任务列表中的任务都使用了正确的异步执行方式：使用一个 callback，或者返回一个 promise 或 stream。

- **fn**：
 该函数定义任务所要执行的一些操作。通常来说，它会是这种形式：`gulp.src().pipe(someplugin())`。

**异步任务支持**  
任务可以异步执行，如果 `fn` 能做到以下其中一点：
1. **接受一个 callback**

```js
// 在 shell 中执行一个命令
var exec = require('child_process').exec;
gulp.task('jekyll', function(cb) {
  // 编译 Jekyll
  exec('jekyll build', function(err) {
    if (err) return cb(err); // 返回 error
    cb(); // 完成 task
  });
});
```

2. **返回一个 stream**

```js
gulp.task('somename', function() {
  var stream = gulp.src('client/**/*.js')
    .pipe(minify())
    .pipe(gulp.dest('build'));
  return stream;
});
```

3. **返回一个 promise**

```js
// Promise库
var Q = require('q');

gulp.task('somename', function() {
  var deferred = Q.defer();

  // 执行异步的操作
  setTimeout(function() {
    deferred.resolve();
  }, 1);

  return deferred.promise;
});

```

**注意**：
默认的，task 将以最大的并发数执行，也就是说，gulp 会一次性运行所有的 task 并且不做任何等待。如果你想要创建一个序列化的 task 队列，并以特定的顺序执行，你需要做两件事：
- 给出一个提示，来告知 task 什么时候执行完毕
- 并且再给出一个提示，来告知一个 task 依赖另一个 task 的完成

```js
var gulp = require('gulp');

// 返回一个 callback，因此系统可以知道它什么时候完成
gulp.task('one', function(cb) {
    // 做一些事 -- 异步的或者其他的
    cb(err); // 如果 err 不是 null 或 undefined，则会停止执行，且注意，这样代表执行失败了
});

// 定义一个所依赖的 task 必须在这个 task 执行之前完成
gulp.task('two', ['one'], function() {
    // 'one' 完成后
});

gulp.task('default', ['one', 'two']);

```

### gulp.watch(glob [, opts], tasks) 或 gulp.watch(glob [, opts, cb])
监视文件，并且可以在文件发生改动时候做一些事情。它总会返回一个 `EventEmitter` 来发射（emit） `change` 事件。

**参数**：
- **glob**：(String/Array)  
一个 glob 字符串，或者一个包含多个 glob 字符串的数组，用来指定具体监控哪些文件的变动  
**注意**：  
1.路径不能使用`./xx`，否则监听不到文件添加  
2.不能同步文件的删除 

- **opts**：(Object)  
传给 `gaze` 的参数

- **tasks**：(Array)  
需要在文件变动后执行的一个或者多个通过 gulp.task() 创建的 task 的名字

```js
var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
watcher.on('change', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```
- **cb(event)**：(Function)  
每次变动需要执行的 callback
    - **event**：  
    描述了所监控到的变动的event对象
        - **event.type**：(String)
        发生的变动的类型：`added`, `changed` 或者 `deleted`
        
        - **event.path**：(String)
        触发了该事件的文件的路径

```js
gulp.watch('js/**/*.js', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```
**处理报错**

在可能产生报错然后推出程序的处理后加上如下error事件处理，防止推出程序

```js
gulp.watch( babel_config.src , function (event) {
    // 只编译改变的文件
    gulp.src(event.path)
    .pipe(babel({
        presets: ['es2015']
    }))
    .on('error',function(error){
          console.log(error)
          //防止错误影响后续命名执行
          this.emit('end');
    })
    .pipe(gulp.dest(babel_config.dest));
});

```


