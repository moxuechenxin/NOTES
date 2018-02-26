## 相关属性（IE8+）
- **counter-reset**
- **counter-increment**
- **content: counter()**
- **content: counters()**

## 属性详解
### 初始化计数器
```css
/**
 * section为计数器名字 
 * 1为计数器起始数（默认为0）
 */
ol {
    counter-reset: section 1;
}

/**
 * 可以多个计数器同时命名（空格分隔）
 */
ol {
    counter-reset: section 1 chapter 2;
}
```

### 计数器自增
```css
/* 碰到li就自增, 2表示每次计数的变化值（默认为1，可为负） */
ol li { 
    counter-increment: section 2; 
}
/* 可以同时设置多个计数器（空格分隔） */
ol li {
    counter-increment: section 2 chapter 3;
}
```
**普照规则**：  
普照源(`counter-reset`)唯一，每普照(`counter-increment`) 1次，普照源增加1次计数值


### 显示计数器
显示计数器有2种方式，`counter`和`counters`
#### counter
`counter`只是简单的从前至后计数，忽略嵌套

**语法**：  
```css
counter( <counter-name> [, <counter-style>] )
```
**参数**：  
- `counter-name`：计数器名称
- `counter-style`：计数器显示风格，默认值为`decimal`，可选值：与`list-style-type`基本一致（除：`circle`，`aquare`，`lower-alpha`，`upper-mongolian`）

**例子**：
```css
ol li:before { 
    content: counter(section, upper-roman) ". ";
}

/* 一个content属性值可以有多个counter()方法 */
.counter:before { 
    content: counter(section) '\A' counter(chapter);
    white-space: pre;
}
```

#### counters
嵌套计数

**语法**：
```css
counters(计数器名称, 嵌套时拼接字符串[, 可选的显示风格])
```
**例子**：

使用`.`分割
```
ol li:before { 
    content: counters(section, "."); 
}
```
将`counters`或者`counter`与任意字符串用空格拼接
```
ol li:before { 
    content: "我是字符串1" counters(section, ".") "我是字符串2" "我是字符串3"; 
}
```
`counter`和`counters`混用
```
ol li:before { 
    content: counter(section) ". " counters(section, ".", lower-alpha) ". "; 
}
```

```html
<style>
    .reset { 
        padding-left: 20px; 
        counter-reset: wangxiaoer;
        line-height: 1.6; 
        color: #666; 
    }
    .counter:before { 
        content: counters(wangxiaoer, '-') '. '; 
        counter-increment: wangxiaoer; 
        font-family: arial black; 
    }
</style>
<div class="reset">
    <div class="counter">我是王小二
        <div class="reset">
            <div class="counter">我是王小二的大儿子</div>
            <div class="counter">我是王小二的二儿子
                <div class="reset">
                    <div class="counter">我是王小二的二儿子的大孙子</div>
                    <div class="counter">我是王小二的二儿子的二孙子</div>
                    <div class="counter">我是王小二的二儿子的小孙子</div>
                </div>
            </div>
            <div class="counter">我是王小二的三儿子</div>
        </div>
    </div>
    <div class="counter">我是王小三</div>
    <div class="counter">我是王小四
        <div class="reset">
            <div class="counter">我是王小四的大儿子</div>
        </div>
    </div>
```
**注意**：  
一个容器的reset的唯一的，一旦子元素出现reset，会改变整个容器的嵌套关系

**输出结果**
```
1.我是王小二
    1-1.我是王小二的大儿子
    1-2.我是王小二的二儿子
        1-2-1.我是王小二的二儿子的大孙子
        1-2-2.我是王小二的二儿子的二孙子
        1-2-3.我是王小二的二儿子的小孙子
    1-3.我是王小二的三儿子
2.我是王小三
3.我是王小四
    3-1.我是王小四的大儿子
```

## 注意事项
- 一个元素，如果设置了`counter-increment`, 但是其`display`的属性值是`none`或者含有`hidden`属性（针对支持浏览器），则此计数值是不会增加的。而`visibility:hidden`以及其他声明不会有此现象


**参考**：
- [CSS counter计数器(content目录序号自动递增)详解](http://www.zhangxinxu.com/wordpress/2014/08/css-counters-automatic-number-content/)
- [CSS计数器详解](http://blog.liuxianan.com/css-counters.html)