# html hack

```html
<!–[if !IE]><!–> 除IE外都可识别 <!–<![endif]–>

<!–[if IE]> 所有的IE可识别 <![endif]–>

<!–[if IE 6]> 仅IE6可识别 <![endif]–>

<!–[if lt IE 6]> IE6以及IE6以下版本可识别 <![endif]–>

<!–[if gte IE 6]> IE6以及IE6以上版本可识别 <![endif]–>

<!–[if IE 7]> 仅IE7可识别 <![endif]–>

<!–[if lt IE 7]> IE7以及IE7以下版本可识别 <![endif]–>

<!–[if gte IE 7]> IE7以及IE7以上版本可识别 <![endif]–>

<!–[if IE 8]> 仅IE8可识别 <![endif]–>

<!–[if IE 9]> 仅IE9可识别 <![endif]–>
```

符号 | 范例 | 说明
---|---|---
! |	[if !IE] | NOT运算符。这是摆立即在前面的功能，操作员，或子表达式扭转布尔表达式的意义
lt | [if lt IE 5.5] | 小于运算符。如果第一个参数小于第二个参数，则返回true
lte | [if lte IE 6] | 小于或等于运算。如果第一个参数是小于或等于第二个参数，则返回true
gt | [if gt IE 5] | 大于运算符。如果第一个参数大于第二个参数，则返回true
gte | [if gte IE 7] | 大于或等于运算。如果第一个参数是大于或等于第二个参数，则返回true
( ) | [if !(IE 7)] | 子表达式运营商。在与布尔运算符用于创建更复杂的表达式
& | [if (gt IE 5)&(lt IE 7)] | AND运算符。如果所有的子表达式计算结果为true，返回true
\| | [if (IE 6)\|(IE 7)] | OR运算符。返回true，如果子表达式计算结果为true


# css hack
## 1. IE6 css hack:
```css
*html Selector {} /* Selector 表示 css选择器 下同 */

Selector { _property: value; } /* property: value 表示 css 的属性名: 属性值 下同 */

Selector { _property/**/: /**/value; }

Selector { -property: value; } /*IE6 css hack常用习惯推荐使用下划线_ */
```
## 2. IE7 css hack
```css
*+html Selector {}

*:first-child+html Selector {}
```

## 3. IE8 css hack
```css
Selector { /* 注意看value值的 */
    property: value1; /* W3C MODEL */
    property: value2\0; /* IE 8+ */
    property: value1\9\0; /* IE 9+ */
}
```
## 4. IE6、IE7、IE8共有的css hack
```css
Selector { property: value\9; }
```
## 5. IE6、IE7共有的css hack
```css
Selector { *property: value; }

Selector { #property: value; }

Selector { +property: value; }
```
## 6. IE8+ css hack
```css
Selector { property: value\0; }
```
## 7. IE9+ css hack
```css
Selector { property: value\9\0; }
```

**参考**:
- [Css Hack 大全（IE6、IE7、IE8、IE9 css hack）](http://www.webhj.com/hj-650.html)