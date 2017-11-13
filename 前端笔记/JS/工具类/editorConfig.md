## 介绍
EditorConfig是一套用于统一代码格式的解决方案，可以帮助开发者在不同的编辑器和IDE之间定义和维护一致的代码风格

## 配置文件
当打开一个文件时，EditorConfig插件会在打开文件的目录和其每一级父目录查找`.editorconfig`文件，直到有一个配置文件`root=true`。  
EditorConfig配置文件从上往下读取，并且路径最近的文件最后被读取。匹配的配置属性按照属性应用在代码上，所以最接近代码文件的属性优先级最高。

### 配置文件格式
EditorConfig文件使用INI格式，允许在分段名（section names）中使用`and`，分段名是全局的文件路径，格式类似于gitignore  
斜杠(`/`)作为路径分隔符，`#`或者`;`作为注释。注释应该单独占一行。EditorConfig文件使用UTF-8格式、CRLF或LF作为换行符

**通配符**：
通配符 | 说明
---|---
`*` | 匹配除/之外的任意字符串
`**` | 匹配任意字符串
`?` | 匹配任意单个字符
`[name]` |	匹配name字符
`[!name]`	| 匹配非name字符
`{s1,s3,s3}` | 匹配任意给定的字符串（0.11.0起支持）
特殊字符可以用\转义，以使其不被认为是通配符

**支持的属性**：
- **root**：表明是最顶层的配置文件，发现设为true时，才会停止查找.editorconfig文件

- **charset**：编码格式  
支持latin1、utf-8、utf-8-bom、utf-16be和utf-16le，不建议使用uft-8-bom

- **indent_style**：
  - `tab`：hard-tabs
  - `space`：soft-tabs

- **indent_size**：设置整数表示规定每级缩进的列数和soft-tabs的宽度（译注：空格数）。如果设定为tab，则会使用tab_width的值（如果已指定

- **tab_width**：设置整数用于指定替代tab的列数。默认值就是indent_size的值，一般无需指定

- **end_of_line**：定义换行符，支持lf、cr和crlf

- **trim_trailing_whitespace**：设为true表示会除去换行行首的任意空白字符，false反之

- **insert_final_newline**：设为true表明使文件以一个空白行结尾，false反之

**参考**：
- [【译】EditorConfig介绍](http://www.alloyteam.com/2014/12/editor-config/)