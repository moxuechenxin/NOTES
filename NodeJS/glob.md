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