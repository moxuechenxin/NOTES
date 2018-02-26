### nodeType
nodeType | 节点类型 | 描述
---|---|---
Element | 1 | 代表元素
Attr | 2 | 代表属性
Text | 3 | 代表元素或属性中的文本内容
Comment | 8 | 代表注释
Document | 9 | 代表整个文档（DOM 树的根节点）

### 节点树和元素树

查询关系节点（元素）| 结点树 | 元素树 (IE9+)
---|---|---
父节点 | parentNode | parentElement
全部子节点 | childNodes | children
第一个子节点 | firstChild | firstElementChild
最后一个子节点 | lastChild | lastElementChild
下一个兄弟 | nextSibling | nextElementSibling
上一个兄弟 | previousSibling | previousElementSibling
