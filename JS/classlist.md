## 说明
`Element.classList` 是一个只读属性，返回一个元素的类属性的实时 `DOMTokenList`集合。
使用 `classList` 是一个方便的替代方法，通过`element.className`作为空格分隔的字符串访问元素的类列表

## 属性
### length
表示元素类名的个数，只读

## 方法
### add( String [, String] )
添加指定的类值。如果这些类已经存在于元素的属性中，那么它们将被忽略。

### remove( String [,String] )
删除指定的类值

### item ( Number )
按集合中的索引返回类值

### toggle ( String [, force] )
当只有一个参数时：切换 class value; 即如果类存在，则删除它并返回false，如果不存在，则添加它并返回true。  
当存在第二个参数时（IE不支持）：如果第二个参数的计算结果为true，则添加指定的类值，如果计算结果为false，则删除它

### contains( String )
检查元素的类属性中是否存在指定的类值

**参考**：
- [Element.classList](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/classList)