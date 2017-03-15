# String对象属性
## 静态属性
属性 | 描述
--|--
String.prototype |	使您有能力向对象添加属性和方法。
String.length | 返回`1`

## 实例属性
属性 | 描述
--|--
constructor	| （继承自String.prototype.constructor）返回对创建此对象的函数的引用。
length | 返回字符串中字符编码单元的数量

**注意**：JavaScript 使用 UTF-16 编码，该编码使用一个 16 比特的编码单元来表示大部分常见的字符，使用两个代码单元表示不常用的字符。因此 `length` 返回值可能与字符串中实际的字符数量不相同。

# String对象方法
## 静态方法
方法 | 参考 | 描述
--|--|--
String.fromCharCode() | (num1[, ...[, numN]]) | 返回使用指定的Unicode值序列创建的字符串
String.fromCodePoint() | (num1[, ...[, numN]]) |（ES6）返回使用指定的代码点序列创建的字符串
String.raw() | (callSite, ...substitutions) |（ES6）返回一个模板字符串的原始字面量值 

## 实例方法
**字符查找**
方法 | 参考 | 描述
--|--|--
charAt() | (index) | 返回字符串中指定位置的字符（等同于通过下标索引取值）
charCodeAt() | (index) | 返回指定索引位置字符的Unicode值
codePointAt() | (index) |（ES6）返回指定索引位置字符的Unicode值
indexOf() | (searchValue[, fromIndex]) | 返回从指定位置起指定值第一次出现的位置索引（`fromIndex`<0等同于0），如果没有找到返回-1
lastIndexOf() | (searchValue[, fromIndex]) | 同上，但查找方向从右到左
includes() | (searchValue[, fromIndex]) |（ES6）判断一个字符串是否包含在另一个字符串中,返回`true`或`false`

**字符串截取**
方法 | 参考 | 描述
--|--|--
slice() | (start[, end]) | 返回截取的部分字符串（`start`和`end`均可取负）
substr() | (start[, length]) | 返回字符串中从指定位置开始到指定长度的子字符串（`start`可取负）
substring() | (start[, end]) |返回字符串两个索引之间（或到字符串末尾）的子串

**正则相关方法**
方法 | 参考 | 描述
--|--|--
search() | (regexp \| substr) | 返回正则表达式在字符串中首次匹配项的索引，无匹配则返回-1
match() | (regexp \| substr) | 返回一个包含匹配结果的数组，无匹配则返回null 
replace() | (regexp \| substr , newSubStr \| function) | 返回替换后的字符串,无匹配则返回原字符串
split() | ([separator][, limit]) | 将字符串拆分为一个字符串数组



**其他方法**
方法 | 参考 | 描述
--|--|--
toLowerCase() | 无 | 转小写
toUpperCase() | 无 | 转大写
trim() | 无 | （IE9+）返回删除了两端空白字符的新字符串
concat() | (string2[, ..., stringN]) | 将一个或多个字符串与原字符串连接合并，形成一个新的字符串并返回
link() | (url) | 创建一个 <a> HTML 元素，用该字符串作为超链接的显示文本,返回字符串:`<a href='url'>str</a>`

# String方法详解
## 实例方法
### substring()
返回字符串两个索引之间（或到字符串末尾）的子串

**语法**：
```js
str.substring(start[, end])
```
**参数**：
- **start**：截取的起始位置
- **end**：截取的结束位置（不包含）

**注意**：
- 如果任一参数小于 `0` 或为 `NaN`，则被当作 `0`
- 如果任一参数大于 `length`，则被当作 `length`
- 如果 `start` 大于 `end`，则 `substring` 的执行效果就像两个参数调换了一样

