## 与js异同
- nodejs包含ecmascript
- nodejs的全局对象为 **global**,且声明变量后不能在通过global的对应属性获取到
```js
var a = 1; //声明的变量属于当前模块（文件）
console.log(a);  // undefined

var b = 2;
global.b = 3;
console.log(b); //2
consoel.log(global.b) //3
```

## 模块
### require
#### 路径:
- **相对路径**    
  注意：同级目录要用'./xx.js'不能使用'xx.js',
 后一种写法表示加载node中的核心模块，或者是node_modules
- **绝对路径**

#### 文件查询规则
1. 先找同名文件
2. 未找到则找同名加'.js'结尾的文件
3. 未找到则找同名加'.json'结尾的文件
4. 未找到则找同名加'.node'结尾的文件，还未找到则报错

## process

