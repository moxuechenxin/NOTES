- #### Cookie
  - 数据存储到计算机中，通过浏览器控制添加与删除数据
- #### Cookie的特点
    - 存储限制  
    - 域名100个cookie,每组值大小4KB  
    - 客户端、服务器端，都会请求服务器（头信息）  
    - 页面间的cookie是共享  

- #### Storage
  - #####  sessionStorage
    session临时回话，从页面打开到页面关闭的时间段
  窗口的临时存储，页面关闭，本地存储消失
  - ##### localStorage
    永久存储（可以手动删除数据）
- #### Storage的特点
    - 存储量限制 ( 5M )
    - 客户端完成，不会请求服务器处理
    - sessionStorage数据是不共享、 localStorage共享
- #### Storage API
    - ##### setItem():
        设置数据，key\value类型，类型都是字符串
        可以用获取属性的形式操作
    - ##### getItem():
        获取数据，通过key来获取到相应的value
    - ##### removeItem():
        删除数据，通过key来删除相应的value
    - ##### clear():
        删除全部存储的值
- #### Storage API 2
    - ##### 存储事件:
        \-当数据有修改或删除的情况下，就会触发**storage**事件  

        \-在对数据进行改变的窗口对象上是不会触发的
        
        \-**Key** : 修改或删除的key值，如果调用clear(),key为null
        
        \-**newValue**  :  新设置的值，如果调用removeStorage(),key为null
        
        \-**oldValue** :  调用改变前的value值
        
        \-**storageArea** : 当前的storage对象
        
        \-**url** :  触发该脚本变化的文档的url
        
        \-注：session同窗口才可以,例子：iframe操作
        

```
<body>
	<input type="checkbox" name="fruit" value="苹果">苹果<br/>
	<input type="checkbox" name="fruit" value="梨子">梨子<br/>
	<input type="checkbox" name="fruit" value="香蕉">香蕉<br/>
	<input type="checkbox" name="fruit" value="桃子">桃子<br/>
	<script>
		var inputs=document.getElementsByTagName("input");
		var len=inputs.length;
		for(var i=0;i<len;i++){
			inputs[i].onclick=function(e){
				if(this.checked){
					window.localStorage.setItem("sel",this.value);
				}else{
					window.localStorage.setItem("nosel",this.value);
				}
			}
		}
		window.addEventListener("storage",function(e){
			console.log(1111);
			if(e.key==="sel"){
				for(var i=0;i<len;i++){
					if(inputs[i].value===e.newValue){
					  inputs[i].checked=true;
					}
				}
			}else if(e.key==="nosel"){
				for(var i=0;i<len;i++){
					if(inputs[i].value===e.newValue){
					  inputs[i].checked=false;
					}
				}
			}
		})
	</script>
</body>

```
**参考**:
- https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API


