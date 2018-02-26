## 创建元素或节点
### document.`createElement`()
创建元素节点。

### document.`createTextNode`()
创建文本节点。

### document.`createAttribute`()
创建属性节点。

### node.`cloneNode`(deep)
创建并返回节点的拷贝。`deep`设置为 `true`，如果您需要克隆节点及其属性，以及后代；设置为 `false`（默认值），如果您只需要克隆节点及其后代


## 插入/删除/替换元素或节点
### parentNode.`appendChild`(node)
把新的子节点添加到指定节点，作为最后一个子节点。**返回被添加的节点**

### existingParentNode.`insertBefore`(newNode [, existingNode])
在指定的子节点前面插入新的子节点，**返回插入的节点**。如果`existingNode`未指定，则 `insertBefore` 方法会在结尾插入 `newnode`。

### parentNode.`removeChild`(node)
删除子节点。**返回被删除的子节点**。

### parentNode.`replaceChild`(newNode, oldNode)
替换子节点。**返回被替换的节点`oldNode`**。

## 属性操作
### node.`getAttribute`(name)
返回指定的属性值

### node.`setAttribute`(name, value)
把指定属性设置或修改为指定的值。无返回值。

### node.`removeAttribute`(name)
删除指定的属性。无返回值。

## 其他方法
### node.`normalize`()
移除空的文本节点，并连接相邻的文本节点。无返回值。

### node1.`compareDocumentPosition`(node2)
（IE9+）返回一个数值，表示两个节点彼此做比较的位置：
- `1`：没有关系，两个节点不属于同一个文档。
- `2`：第一节点（node1）位于第二个节点后（node2）。
- `4`：第一节点（node1）定位在第二节点（node2）前。
- `8`：第一节点（node1）位于第二节点内（node2）。
- `16`：第二节点（node2）位于第一节点内（node1）。
- `32`：没有关系，或是两个节点是同一元素的两个属性。
