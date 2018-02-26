## JSON对象
JSON 是一种用来序列化对象、数组、数值、字符串、布尔值和 null 的语法。它基于 JavaScript 语法，但是又有区别：一些 JavaScript 值不是 JSON，而某些 JSON 不是 JavaScript 值。

JavaScript类型 |  JSON与之区别
---|---
对象和数组 |  属性名称必须用双引号包裹；最后一个属性后面不能有逗号。
数值 |	前导0不能使用（在 JSON.stringify 中将会被忽略，在 JSON.parse 会抛出错误）；小数点后面至少有一个数字。
字符串	| 只有有限的字符能够被转义；不允许某些控制字符；但允许使用Unicode 行分隔符 (U+2028) 和段落分隔符 (U+2029) ; 字符串必须用双引号括起来。

**注意**：
- **JSON中，属性的值仅允许字符串，数字，数组，true，false，或者其他JSON对象**
- 空白字符可以出现在任意位置，但是数值类型的数字中间不能有空白字符，字符串中间不能随意添加空白字符，因为添加的空白字符会被解释为相应的字符值，从而引起错误(译者注:因此在解析JSONString字符串常见的一个错误就是当要解析的JSONString中出现字符\f\n\r\t\v是会报错的,在解析出现上述字符的JSONString的时候请首先将其转化为其他编码格式如HTML-code编码格式(例如&#32; 不完全支持),或者干脆直接将上述字符去掉)
- 有效空白字符只包括制表符 (U+0009)、回车符 (U+000D)、换行符 (U+000A) 和空格 (U+0020) 。

## 方法
### JSON.parse()  IE8+ (ES5)
将一个 JSON字符串解析成一个javascript值，在解析过程中，还可以选择性的修改某些属性的原始解析值

#### 语法
`JSON.parse(text[, reviver])`
#### 参数
**text**  
要被解析成JavaSctipt值的字符串，查看 JSON 对象学习的JSON 语法的说明。 

**reviver** 可选  
如果是一个函数，则规定了原始值如何被解析改造，在被返回之前。

#### 使用reviver函数
如果指定了 reviver 函数，则解析出的 JavaScript 值（解析值）会经过一次转换后才将被最终返回（返回值）

**具体过程**：  
1. 解析值本身以及它所包含的所有属性，会按照一定的顺序（从最最里层的属性开始，一级级往外，最终到达顶层，也就是解析值本身）分别的去调用 reviver 函数
2. 在调用过程中，当前属性所属的对象会作为 this 值，当前属性名和属性值会分别作为第一个和第二个参数传入 reviver 中。如果 reviver 返回 undefined，则当前属性会从所属对象中删除，如果返回了其他值，则返回的值会成为当前属性新的属性值
3. 当遍历到最顶层的值（解析值）时，传入 reviver 函数的参数会是空字符串 ""（因为此时已经没有真正的属性）和当前的解析值（有可能已经被修改过了），当前的 this 值会是 {"": 修改过的解析值}，在编写 reviver 函数时，要注意到这个特例。（译者按：这个函数的遍历顺序按深度优先遍历）

```
JSON.parse('{"1": 1, "2": 2,"3": {"4": 4, "5": {"6": 6}}}', function (k, v) {
    console.log(k); // 输出当前的属性名，从而得知遍历顺序是从内向外的，
                    // 最后一个属性名会是个空字符串。
    return v;       // 返回原始属性值，相当于没有传递 reviver 参数。
});

// 1
// 2
// 4
// 6
// 5
// 3 
// 
// { 1: 1, 2: 2, 3: { 4: 4,5: {6: 6} } }
```
**注意**：  
**JSON.parse() 不允许用逗号作为结尾**

```js
// both will throw a SyntaxError
JSON.parse("[1, 2, 3, 4, ]");
JSON.parse('{"foo" : 1, }');
```


### JSON.stringify() IE8+ (ES5)
将任意的 JavaScript 值序列化成 JSON 字符串。  
（若转换的函数被指定，则被序列化的值的每个属性都会经过该函数的转换和处理；若转换的数组被指定，只有包含在这个数组中的属性名才会被序列化到最终的 JSON 字符串中）

#### 语法
`JSON.stringify(value[, replacer [, space]])`
#### 参数
**value**  
将要序列化成 JSON 字符串的值。  

**replacer** 可选  
1. 如果该参数是一个函数，则在序列化过程中，被序列化的值的每个属性都会经过该函数的转换和处理；
2. 如果该参数是一个数组，则只有包含在这个数组中的属性名才会被序列化到最终的 JSON 字符串中；
3. 如果该参数为null或者未提供，则对象所有的属性都会被序列化；
 

**space** 可选  
指定缩进用的空白字符串，用于美化输出（pretty-print）：  
1. 如果参数是个数字，它代表有多少的空格；上限为10。该值若小于1，则意味着没有空格；
2. 如果该参数为字符串(字符串的前十个字母)，该字符串将被作为空格；
3. 如果该参数没有提供（或者为null）将没有空格。

#### 注意事项

- 非数组对象的属性不能保证以特定的顺序出现在序列化后的字符串中。
- 布尔值、数字、字符串的包装对象在序列化过程中会自动转换成对应的原始值。
- **undefined**、任意的**函数**以及 **symbol** 值，在序列化过程中会被忽略（出现在非数组对象的属性值中时）或者被转换成 null（出现在数组中时）。
- 所有以 symbol 为属性键的属性都会被完全忽略掉，即便 replacer 参数中强制指定包含了它们。
- 不可枚举的属性会被忽略

#### replacer参数
replacer参数可以是一个函数或者一个数组。

##### 作为函数，它有两个参数，键(key)值(value)都会被序列化。

- 如果返回一个 Number, 转换成相应的字符串被添加入JSON字符串。
- 如果返回一个 String, 该字符串作为属性值被添加入JSON。
- 如果返回一个 Boolean, "true" 或者 "false"被作为属性值被添加入JSON字符串。
- 如果返回任何其他对象，该对象递归地序列化成JSON字符串，对每个属性调用replaceer方法。除非该对象是一个函数，这种情况将不会被序列化成JSON字符串。
- 如果返回undefined，该属性值不会在JSON字符串中输出。  

**注意**: 不能用replacer方法，从数组中移除值(values)，如若返回undefined或者一个函数，将会被null取代。

```js
var foo = {foundation: "Mozilla", model: "box", week: 45, transport: "car", month: 7};
var jsonString = JSON.stringify(foo, function(key, value) {
  if (typeof value === "string") {
    return undefined;
  }
  return value;
});  // {"week":45,"month":7}

```
##### 如果replacer是一个数组，数组的值代表将被序列化成JSON字符串的属性名。

```js
var foo = {foundation: "Mozilla", model: "box", week: 45, transport: "car", month: 7};
JSON.stringify(foo, ['week', 'month']);  
// '{"week":45,"month":7}', 只保留“week”和“month”属性值。
```

#### space 参数
space 参数用来控制结果字符串里面的间距 

1. 如果是一个数字, 则在字符串化时每一级别会比上一级别缩进多这个数字值的空格（最多10个空格）；
2. 如果是一个字符串，则每一级别会比上一级别多缩进用该字符串（或该字符串的前十个字符）。


```js
JSON.stringify({ a: 2 }, null, " ");   // '{\n "a": 2\n}'
```

```js
// 使用制表符（\t）来缩进
JSON.stringify({ uno: 1, dos : 2 }, null, '\t')
// '{            \
//     "uno": 1, \
//     "dos": 2  \
// }'
```

#### toJSON 方法
如果一个被序列化的对象拥有 toJSON 方法，那么该 toJSON 方法就会覆盖该对象默认的序列化行为：不是那个对象被序列化，而是调用 toJSON 方法后的返回值会被序列化  

```js
var obj = {
  foo: 'foo',
  toJSON: function () {
    return 'bar';
  }
};
JSON.stringify(obj);      // '"bar"'
JSON.stringify({x: obj}); // '{"x":"bar"}'
```



**参考**：
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON