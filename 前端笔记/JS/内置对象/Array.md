## 创建数组
1. **字面量**

```js
var arr = [1,2,3];
```

2. **调用构造函数**  
可以不使用`new`直接调用`Array()`

```js
var arr1 = new Array(); // [ ]

var arr2 = new Array(2); //[undefined,undefined]

var arr3 = new Arra(1,2,3); // [1,2,3]
```

## Array对象属性
### 静态属性
属性 | 描述
---|---
Array.prototype |	使您有能力向对象添加属性和方法。

### 实例属性
属性 | 描述
---|---
constructor	| （继承自Array.prototype.constructor）返回对创建此对象的数组函数的引用。
length | 设置或返回数组中元素的数目。

## Array对象方法
### 静态方法
方法 | 描述
---|---
Array.isArray() | (IE9+) 判断是否为数组对象
Array.from() | (ES6) 将类数组对象或可遍历对象转为真正的数组
Array.of() | (ES6) 将一组值（参数列表）转为数组

**注意**:
- `Array.from({length: 2})`和`[].slice.call({length: 2})`返回值不同，前者返回值为长度为2，每项值为undefined（可用于forEach等）；后者返回值为长度为2，每项未定义（不可用于forEach等）

### 实例方法
### 1. 增删 (直接修改原数组)
方法 | 参数 |描述
---|---|---
push() | (item1,item2...)	| 向数组的末尾添加一个或更多元素 (返回新的长度)。
unshift() |	(item1,item2...）	| 向数组的开头添加一个或更多元素 (返回新的长度)。
pop() |	无 | 删除数组的最后一个元素 (返回删除的元素)。
shift()	| 无 | 删除数组的第一个元素 (返回删除的元素)。

### 2. 其他（直接修改原数组）的方法
方法 | 参数 |描述
---|---|---
sort() | ( [ function (a,b){ ...; return ...} ] )	| 对数组的元素进行排序 (返回原数组)。
reverse() |	无 | 颠倒数组中元素的顺序 (返回原数组)。
splice() | (start,count[,item1,...itemN])  `start可取负数` | 	删除元素，并向数组添加新元素（返回被删除的数组项组成的数组）。
fill() | (value[, start = 0[, end = this.length]]) `start和end可取负数` | (ES6) 将一个数组(类数组)中指定区间的所有元素的值, 都替换成或者说填充成为某个固定的值

### 3. 不修改原数组
方法 | 参数 |描述
---|---|---
slice()	| ( [start [,end]] ) `start和end可取负数` |从某个已有的数组返回选定的元素组成的新数组
concat() | (arr1,arr2...)	| 连接两个或更多的数组，并返回结果。
join() | ( [separator] ) 默认为"," 	| 把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔。
toSource() | |	返回该对象的源代码。
toString() | | 	把数组转换为字符串，并返回结果。
toLocaleString() | | 	把数组转换为本地数组，并返回结果。
valueOf() |	| 返回数组对象的原始值

### 4.遍历
(ES5) 参数：( function (item,index,array), context )

方法 | 描述
---|---
forEach() | 遍历数组，(无返回值)
map() | 遍历数组，(返回一个由每次遍历函数返回值组成的新数组)
filter() | 遍历数组，(返回一个由每次遍历函数返回值为true的项组成的新数组)
some() | 遍历数组，(任意次遍历函数返回true，则该方法返回true)
every() | 遍历数组，(全部遍历函数返回true，该方法才返回true)

**注意**：
- 遍历时未定义的数组项会被跳过（注意数组项为undefined和未定义的区别）
```
// arr1每项都没定义
var arr1 = new Array(3);

// arr2每项都为undefined
var arr2 = Array.apply(null, Array(3));

```

### 5.迭代
(ES5) 参数：( function (prev,current,index,array), startVal )
方法 | 描述
---|---
reduce |  从数组起始位置开始迭代（返回累积操作的结果）
reduceRight | 从数组末尾开始迭代（返回累积操作的结果）

### 6.查找
方法 | 参数 | 描述
---|---|---
indexOf() | (searchElement [, fromIndex = 0]) |(ES5) 查找给定的元素位置（返回第一次出现的索引值，如果没找到则返回-1）
findIndex() | (function (item,index,array){} [, context] ) | (ES6) 查找符合函数条件（返回true）的元素的位置（返回第一次出现的索引值，如果没找到则返回-1）
find() | (function (item,index,array){} [, context] ) | (ES6) 查找满足函数条件（返回true）的元素 (返回符合的第一个元素，如果没有则返回undefined)
includes() | (searchElement[, fromIndex]) | (ES7) 判断当前数组是否包含某指定的值，如果是，则返回 true，否则返回 false




## Array方法详解
### 静态方法
#### Array.isArray()
(IE9+，ES5) 判断是否为数组

```js
Array.isArray([1,2]) // true
```
**Ployfill**

```js
if (!Array.isArray) {
  Array.isArray = function(arg) {
    return Object.prototype.toString.call(arg) === '[object Array]';
  };
}
```
#### Array.from()
(ES6) 将类数组对象和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）转为真正的数组

```js
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ES5的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']

// ES6的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```

#### Array.of()
(ES6) 将一组值（参数列表）转为数组

```js
Array(3) // [undefined,undefined,undefined]
Array(3, 11, 8) // [3,11,8]

Array.of(3) // [3]
Array.of(3, 11, 8) // [3,11,8]
```

### 实例方法
#### sort()
对数组的元素做原地的排序，(原数组被修改) (返回原数组)  
**语法**：

```js
arr.sort([compareFunction])
```
**参数**：
- **compareFunction**  
可选。用来指定按某种顺序进行排列的函数。如果省略，元素按照转换为的字符串的诸个字符的Unicode位点进行排序 (按字符串从左往右比较排序，数字会转为字符串)。

```js
// 升序（数字）
function ascend(a,b){
  return a - b;
}
// 如果是字符串或数字
function ascend(a,b){
  if(a>b){
    return 1;
  }else if(a<b){
    return -1;
  }
  return 0;
}
// 降序（数字）
function descend(a,b){
  return b - a;
}

var arr = [1,4,2,0,3];
arr.sort(ascend);  // [0,1,2,3,4]
arr.sort(descend);  // [4,3,2,1,0]

```
#### reverse()
颠倒数组中元素的位置。第一个元素会成为最后一个，最后一个会成为第一个。(原数组被修改) (返回原数组)    

**语法**：

```js
arr.reverse()
```

```js
var arr = ['one', 'two', 'three'];
arr.reverse(); // ['three', 'two', 'one']
```
#### splice()
用新元素替换旧元素，以此修改数组的内容，(原数组被修改) (返回由被删除的元素组成的一个数组)

**语法**：

```js
arr.splice(start, deleteCount[, item1[, item2[, ...]]])
```
**参数**：

- **start**  
 (可取负数) 从数组的哪一位开始修改内容。（如果超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位）。

- **deleteCount**
 要移除的数组元素的个数。（如果 deleteCount 是 0，则不移除元素。这种情况下，至少应添加一个新元素。如果 deleteCount 大于start 之后的元素的总数，则从 start 后面的元素都将被删除（含第 start 位））。

- **itemN**
 要添加进数组的元素。（如果不指定，则 splice() 只删除数组元素）。

```js
// 1. 删除数组项
var arr1 = [1,2,3];
arr1.splice(0,1); // [1]
console.log(arr1); // [2,3]

// 2. 增加数组项
var arr2 = [1,2,3];
arr2.splice(1,0,4); // []
console.log(arr2); // [1,4,2,3]

// 3. 替换数组项
var arr3 = [1,2,3];
arr3.splice(2,1,4); // [3]
console.log(arr3); // [1,2,4]

// 4. 综合运用
var arr4 = [1,2,3];
arr4.splice(0,2,4,5,6); // [1,2]
console.log(arr4); // [4,5,6,3]
```
#### slice()
浅复制数组的一部分到一个新的数组。 (返回截取部分组成的新数组)

**语法**：

```js
arr.slice([start[,end]])
```
**参数**：

- **start**  
 (可取负数) 截取的起始位置。省略该参数则从0开始

- **end**  
 (可取负数) 截取的结束位置(不包含该位置的数组项)。省略该参数则为原数组末尾

```js
var arr1 = [1,2,3];
arr1.slice(); // (浅复制) [1,2,3] 和arr1指向不同的数组对象
arr1.slice(0,2); // [1,2]
```
**将类数组转为数组**
```js
// IE8及以下不能转换dom元素相关的类数组对象：nodeList...
function toArray(arrLike){
    return Array.prototype.slice.call(arrLike);
}
```
**类数组转换Ployfill**
```js
(function () {
    'use strict';
    var _slice = Array.prototype.slice;

    try {
        // Can't be used with DOM elements in IE < 9
        _slice.call(document.documentElement);
    } catch (e) { // Fails in IE < 9
        // This will work for genuine arrays, array-like objects,
        // NamedNodeMap (attributes, entities, notations),
        // NodeList (e.g., getElementsByTagName), HTMLCollection (e.g., childNodes),
        // and will not fail on other DOM objects (as do DOM elements in IE < 9)
        Array.prototype.slice = function (begin, end) {
            // IE < 9 gets unhappy with an undefined end argument
            end = (typeof end !== 'undefined') ? end : this.length;

            // For native Array objects, we use the native slice function
            if (Object.prototype.toString.call(this) === '[object Array]'){
                return _slice.call(this, begin, end);
            }

            // For array like object we handle it ourselves.
            var i, cloned = [],
                size, len = this.length;

            // Handle negative value for "begin"
            var start = begin || 0;
            start = (start >= 0) ? start: len + start;

            // Handle negative value for "end"
            var upTo = (end) ? end : len;
            if (end < 0) {
                upTo = len + end;
            }

            // Actual expected size of the slice
            size = upTo - start;

            if (size > 0) {
                cloned = new Array(size);
                if (this.charAt) {
                    for (i = 0; i < size; i++) {
                        cloned[i] = this.charAt(start + i);
                    }
                } else {
                    for (i = 0; i < size; i++) {
                        cloned[i] = this[start + i];
                    }
                }
            }
            return cloned;
        };
    }
}());
```

#### concat()
方法将传入的数组或非数组值与原数组合并,(返回合并的新数组)

**语法**：

```js
arr.concat( [( val1 , ...valN )] )
```
**参数**：

- **valueN**  
 需要与原数组合并的数组或非数组值

```js
var arr1 = [1,2,3];
// 参数为数组
arr1.concat([4,5]); // [1,2,3,4,5]
// 参数为基本类型
arr1.concat(4); // [1,2,3,4]
// 混合参数
arr1.concat(4,[5,6]); // [1,2,3,4,5,6]
// 不传参数,即浅复制整个数组
arr1.concat();  // [1,2,3]
```
#### reduce()
(ES5) 接收一个函数作为累加器，数组中的每个值（从左到右）开始合并，最终为一个值（返回函数累计处理的结果）。

**语法**：
```js
arr.reduce( callback(prev,current,index,array){} [,initialValue] )
```
**参数**：

- **callback**
  执行数组中每个值的函数，包含四个参数
    - **prev**
    上一次调用回调返回的值，或者是提供的初始值（initialValue）
    - **current**
    当前元素
    - **index**
    当前元素下标
    - **array**
    当前数组
- **initialValue**
  作为第一次调用 callback 的第一个参数

**注意**：

- reduce 为数组中的每一个元素依次执行回调函数，不包括数组中被删除或从未被赋值的元素
- 不提供 initialValue ，reduce 会从索引1的地方开始执行 callback 方法，跳过第一个索引。提供 initialValue ，从索引0开始
- 如果数组为空并且没有提供initialValue， 会抛出TypeError 。如果数组仅有一个元素（无论位置如何）并且没有提供initialValue， 或者有提供initialValue但是数组为空，那么此唯一值将被返回并且callback不会被执行

```js
[0, 1, 2, 3].reduce(function(a, b) {
    return a + b;
}, 0); // 6

// 数组扁平化
[[0, 1], [2, 3], [4, 5]].reduce(function(a, b) {
    return a.concat(b);
}, []); // [0,1,2,3,4,5]
```

## 关于类数组
类数组可以通过`Array.prototype[method]call(arrLike,arg1,arg2...)`或`Array.prototype[method]apply(arrLike,args)`调用数组的方法，效果类似数组

```js
var arrLike = {
    0: "a",
    1: "b",
    length: 2
}
```

```js

// 转为数组
[].slice.call(arrLike)
// 生成长度为20每项值为undefined的数组 (可以用于forEach、map等遍历方法)
Array.apply(null, { length: 20 })
Array.from({length: 20})

// 添加属性2，值为c
// 返回 3（属性length值）
// arrLike => { 0: "a", 1: "b", 2: "c", length: 3 }
[].push.call(arrLike, "c")

// 遍历类数组
// 依次输出: "a", "b", "c"
[].forEach.call(arrLike, function(item, index) {
    console.log(item)
})

```

**参考**：
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array
