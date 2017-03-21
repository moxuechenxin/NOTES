## Cookie
数据存储到计算机中，通过浏览器控制添加与删除数据

##### Cookie的特点
- 存储限制  
- 域名100个cookie,每组值大小4KB  
- 客户端、服务器端，都会请求服务器（头信息）  
- 页面间的cookie是共享  

## Storage (IE8+)
###  Storage 对象：
- **sessionStorage**
    session临时回话，从页面打开到页面关闭的时间段
  窗口的临时存储，页面关闭，本地存储消失

- **localStorage**
    永久存储（可以手动删除数据）

### Storage的特点
- 存储量限制 ( 5M )
- 客户端完成，不会请求服务器处理
- sessionStorage数据是不共享、 localStorage共享

### Storage API
- **setItem()**:
    设置数据，key\value类型，类型都是字符串
    可以用获取属性的形式操作, 即`localStorage[keyName]`

- **getItem()**:
    获取数据，通过key来获取到相应的value

- **removeItem()**:
    删除数据，通过key来删除相应的value
- **clear()**:
    删除全部存储的值

**备注**：可以使用 `Storage.length` 来测试存储对象是否为空

### Storage API 2
#### storage事件:
`Storage` 对象发生变化时（即创建/更新/删除数据项时，重复设置相同的键值不会触发该事件，`Storage.clear()` 方法至多触发一次该事件），`StorageEvent` 事件会触发（**在同一个页面内发生的改变不会起作用——在相同域名下的其他页面（如一个新标签或 `iframe`）发生的改变才会起作用**。在其他域名下的页面不能访问相同的 `Storage` 对象）

**event对象属性**：
- **Key** : 修改或删除的key值，如果调用clear(),key为null

- **newValue**  :  新设置的值，如果调用`removeStorage()`,key为null

- **oldValue** :  调用改变前的value值

- **storageArea** : 触发storage事件的storage对象（即被修改的storage对象）

- **url** :  触发该脚本变化的文档的url

注：session同窗口才可以,例子：iframe操作

```html
<!--
    http://www.a.com/a.html
-->
<body>
	<input type="checkbox" name="fruit" value="苹果">苹果<br/>
	<input type="checkbox" name="fruit" value="梨子">梨子<br/>
	<input type="checkbox" name="fruit" value="香蕉">香蕉<br/>
	<input type="checkbox" name="fruit" value="桃子">桃子<br/>
	<script>
		var inputs = document.getElementsByTagName("input");
		var len = inputs.length;
		for(var i = 0;i<len;i++){
			inputs[i].onclick = function(e){
                var sels = window.localStorage.getItem("sels") ;
                sels = sels === null ? [] : sels.split(",");

				if(this.checked){
                    sels.push(this.value);
				}else{
                    var index = sels.indexOf(this.value);
                    if (index !== -1) {
                        sels.splice(index, 1);
                    }
				}
                window.localStorage.setItem("sels", sels.join(","));
			}
		}
	</script>
</body>

```

```html
<!--
    http://www.a.com/b.html
-->

<script>
    window.addEventListener("storage",function(e){
        console.log(e);
        if(e.key === "sels"){
            console.log('a.html的url为：', e.url);
            console.log('a.html的storage对象为：', e.storageArea);
            console.log('a.html页面之前选中的水果有：', e.oldValue);
            console.log('a.html页面现在选中的水果有：', e.newValue);
        }
    })
</script>

```

**参考**:
- [使用 Web Storage API](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API)
