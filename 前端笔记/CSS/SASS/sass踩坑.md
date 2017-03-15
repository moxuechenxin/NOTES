### 使用 calc 传入变量无效
**错误写法**：
```scss
.class{
    $padding: 5px;
    width: calc(100% - $paddding * 2);
}
```
**正确写法**：
```scss
.class{
    $padding: 5px;
    width: calc(100% - #{$paddding * 2});
}
```
### import语句末尾要加`;`