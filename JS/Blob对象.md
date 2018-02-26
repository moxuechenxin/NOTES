## Blob
在Web中，Blob类型的对象表示不可变的类似文件对象的原始数据，它是类似文件对象的二进制数据，因此可以像操作File对象一样操作Blob对象，实际上，File继承自Blob

## Blob基本用法
### 创建

```js
new Blob(blobParts[, options])
```

**参数**：
`blobParts`：数组类型，数组中的每一项连接起来构成Blob对象的数据，数组中的每项元素可以是`ArrayBuffer`，`ArrayBufferView`，`Blob`，`DOMString`

`options`：可选项，字典格式类型，可以指定如下两个属性
- type：默认值为 ""，它代表了将会被放入到blob中的数组内容的MIME类型
- endings：用于指定包含行结束符\n的字符串如何被写入，有如下两个值
  - transparent（默认值）：表示会保持blob中保存的结束符不变
  - native：表示行结束符会被更改为适合宿主操作系统文件系统的换行符

```js
var data1 = "a";
var data2 = "b";
var data3 = "<div style='color:red;'>This is a blob</div>";
var data4 = { "name": "abc" };

var blob1 = new Blob([data1]);
var blob2 = new Blob([data1, data2]);
var blob3 = new Blob([data3]);
var blob4 = new Blob([JSON.stringify(data4)]);
var blob5 = new Blob([data4]); // 当使用普通对象创建Blob对象时，相当于调用了普通对象的toString()方法得到字符串数据，然后再创建Blob对象
var blob6 = new Blob([data3, data4]);

console.log(blob1);  //输出：Blob {size: 1, type: ""}
console.log(blob2);  //输出：Blob {size: 2, type: ""}
console.log(blob3);  //输出：Blob {size: 44, type: ""}
console.log(blob4);  //输出：Blob {size: 14, type: ""}
console.log(blob5);  //输出：Blob {size: 15, type: ""}
console.log(blob6);  //输出：Blob {size: 59, type: ""}
```

### slice方法
返回一个新的 Blob对象，包含了源 Blob对象中指定范围内的数据

```js
slice([start[, end[, contentType]]])
```
**参数**：

`start`：可选，代表 Blob 里的下标，表示第一个会被拷贝进新的 Blob 的字节的起始位置，可为负
`end`：可选，代表的是 Blob 的一个下标，这个下标对应的字节将会是被拷贝进新的Blob 的最后一个字节
`contentType`：可选，给新的 Blob 赋予一个新的文档类型。这将会把它的 type 属性设为被传入的值

## Blob使用场景
### 分片上传

```js
function uploadFile(file) {
  var chunkSize = 1024 * 1024;   // 每片1M大小
  var totalSize = file.size;
  var chunkQuantity = Math.ceil(totalSize/chunkSize);  //分片总数
  var offset = 0;  // 偏移量

  var reader = new FileReader();
  reader.onload = function(e) {
    var xhr = new XMLHttpRequest();
    xhr.open("POST","http://xxxx/upload?fileName="+file.name);
    xhr.overrideMimeType("application/octet-stream");

    xhr.onreadystatechange = function() {
      if(xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) {
        ++offset;
        if(offset === chunkQuantity) {
          alert("上传完成");
        } else if(offset === chunkQuantity-1){
          blob = file.slice(offset*chunkSize, totalSize);   // 上传最后一片
          reader.readAsBinaryString(blob);
        } else {
          blob = file.slice(offset*chunkSize, (offset+1)*chunkSize);
          reader.readAsBinaryString(blob);
        }
      }else {
        alert("上传出错");
      }
    }

    if(xhr.sendAsBinary) {
      xhr.sendAsBinary(e.target.result);   // e.target.result是此次读取的分片二进制数据
    } else {
      xhr.send(e.target.result);
    }
  }
   var blob = file.slice(0, chunkSize);
   reader.readAsBinaryString(blob);
}
```

### Blob URL
Blob URL是blob协议的URL，它的格式如下：

```js
blob:http://XXX
```

Blob URL可以通过`URL.createObjectURL(blob)`创建。在绝大部分场景下，我们可以像使用Http协议的URL一样，使用Blob URL。常见的场景有：作为文件的下载地址和作为图片资源地址。

> 文件下载地址

```js
function createDownloadFile() {
    var content = "Blob Data";
    var blob = new Blob([content]);
    var link = document.getElementsByTagName("a")[0];
    link.download = "file";
    link.href = URL.createObjectURL(blob);
}
```
点击`<a>`标签，下载一个名为`file`的文件，文件内容为：'Blob DaTa'

> 图片资源地址

```html
<input type="file" accept="image/*" onchange="handleFile(this)" />
<br/>
<img style="width:200px;height:200px">
```

```js
function handleFile(e) {
    var file = e.files[0];
    var blob = URL.createObjectURL(file);
    var img = document.getElementsByTagName("img")[0];
    img.src = blob;
    img.onload = function(e) {
        URL.revokeObjectURL(this.src);  // 释放createObjectURL创建的对象##
    }
}

//  或使用Data URL加载图片资源
function handleFile(e) {
    var file = e.files[0];
    var fileReader = new FileReader();
    var img = document.getElementsByTagName("img")[0];
    fileReader.onload = function(e) {
        img.src = e.target.result;
    }
    fileReader.readAsDataURL(file);
}
```

### Blob URL和Data URL区别
- Blob URL的长度一般比较短，但Data URL因为直接存储图片base64编码后的数据，往往很长

- Blob URL可以方便的使用XMLHttpRequest获取源数据
```js
var blobUrl = URL.createObjectURL(new Blob(['Test'], {type: 'text/plain'}));
var x = new XMLHttpRequest();
// 如果设置x.responseType = 'blob'，将返回一个Blob对象，而不是文本:
// x.responseType = 'blob';
x.onload = function() {
 alert(x.responseText);   // 输出 Test
};
x.open('get', blobUrl);
x.send();
```
对于Data URL，并不是所有浏览器都支持通过XMLHttpRequest获取源数据的

- Blob URL只能在当前应用内部使用，把Blob URL复制到浏览器的地址栏中，是无法获取数据的

为了保证浏览器能正确的解析Blob URL返回的文件类型，需要在创建Blob对象时指定相应的type

```js
// 创建HTML文件的Blob URL
var data = "<div style='color:red;'>This is a blob</div>";
var blob = new Blob([data], { type: 'text/html' });
var blobURL = URL.createObjectURL(blob);

// 创建JSON文件的Blob URL
var data = { "name": "abc" };
var blob = new Blob([JSON.stringify(data)], { type: 'application/json' });
var blobURL = URL.createObjectURL(blob);
```

**参考**：
- [细说Web API中的Blob](https://juejin.im/post/59e35d0e6fb9a045030f1f35)
