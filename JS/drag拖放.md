### draggable ：
– 设置为true，元素就可以拖拽了

### 拖拽元素事件 :  
事件对象为被拖拽元素  
**dragstart**：  拖拽前触发  
**drag**：拖拽前、拖拽结束之间，连续触发  
**dragend**：  拖拽结束触发  

### 目标元素事件 :  
事件对象为目标元素  
**dragenter**：   进入目标元素触发，相当于mouseover  
**dragover**：  进入目标、离开目标之间，连续触发  
**dragleave**：   离开目标元素触发，相当于mouseout  
**drop**：    在目标元素上释放鼠标触发  

### 事件的执行顺序 ：
- **drop不触发的时候**  
dragstart  >  drag >  dragenter >  dragover >  dragleave > dragend
- **drop触发的时候**(dragover的时候阻止默认事件，最好dragenter的时候也阻止默认事件)  
dragstart  >  drag >  dragenter >  dragover >  drop > dragend

### 不能释放的光标和能释放的光标不一样

### 解决火狐下的问题
\- 必须在ondragstart事件处理函数中设置dataTransfer对象才可以拖拽除图片外的其他标签  
\- 取消 drop 事件的默认行为


# dataTransfer对象
**setData()** : 设置数据 key和value(必须是字符串),保存在 dataTransfer 对象中的数据只能在 **drop** 事件处理程序中读取    
**getData()** : 获取数据，根据key值，获取对应的value  
**dropEffect** :   
\- 设置被拖动的元素能够执行哪种放置行为(none,move,copy,link) ,不同值光标显示为不同的符号  
\- 只有搭配 effectAllowed 属性才有用  
**effectAllowed** :  
\- 设置光标样式(none, copy, copyLink, copyMove, link, linkMove, move, all 和 uninitialized)   
\- 必须在 ondragstart 事件处理程序中设置 effectAllowed 属性  
**setDragImage**  
\- 三个参数：指定的元素，坐标X，坐标Y  
**files**  
\- 获取外部拖拽的文件，返回一个filesList列表  
\- filesList下有个type属性，返回文件的类型  

# FileReader(读取文件信息)
**readAsDataURL**  
\- 参数为要读取的文件对象，将文件读取为DataUrl  
**onload**  
\- 当读取文件成功完成的时候触发此事件  
\- this. result , 来获取读取的文件数据，如果是图片，将返回base64格式的图片数据  
**实例**  
\- 拖拽删除列表  
\- 拖拽购物车  
\- 上传图片预览功能  


简易购物车：
```html
<style>
  .goodsWrap{
   border:1px solid #ccc;
   padding:10px;
  }
  .clrfix:after,.clrfix:before{
   content:"";
   display: table;
  }
  .clrfix:after{
   clear:both;
  }
  .clrfix{
   *zoom:1;
  }
  .goodsBox{
   float:left;
   border:1px dotted #ccc;
   margin-right:50px;
   padding:10px;
  }
  img{
   width:100px;
  }
  h4,h5{
   margin:10px 0;
  }
  .cart{
   margin-top:50px;
   width:300px;
   height:200px;
   border: 1px solid #ccc;
  }
  .cartWrap *{
   float:left;
   width:33.333%;
  }
 </style>

<body>
   <div class="goodsWrap clrfix">
    <div class="goodsBox" draggable="true">
     <img src="images/img1.jpg" alt="" draggable="false">
     <h4 class="title">js语言精粹</h4>
     <h5 class="price">￥50</h5>
    </div>
    <div class="goodsBox" draggable="true">
     <img src="images/img2.jpg" alt="" draggable="false">
     <h4 class="title">js权威指南</h4>
     <h5 class="price">￥60</h5>
    </div>
    <div class="goodsBox" draggable="true">
     <img src="images/img3.jpg" alt="" draggable="false">
     <h4 class="title">精通js</h4>
     <h5 class="price">￥70</h5>
    </div>
    <div class="goodsBox" draggable="true">
     <img src="images/img4.jpg" alt="" draggable="false">
     <h4 class="title">js编程艺术</h4>
     <h5 class="price">￥80</h5>
    </div>
   </div>
   <div class="cart">
   </div>
   <script>
    var goodsBox=document.querySelectorAll(".goodsBox");
    [ ].forEach.call(goodsBox,function(item,index,array){
     item.addEventListener("dragstart",function(e){
      //设置dataTransfer对象的数据
      e.dataTransfer.setData("title",item.querySelector(".title").innerHTML);
      e.dataTransfer.setData("price",item.querySelector(".price").innerHTML);
     })
    })
    var cart=document.querySelector(".cart");
    //必须阻止dragover事件的默认行为，否则drop不能触发
    cart.addEventListener("dragover",function(e){
     e.preventDefault();
    })
    cart.addEventListener("drop",function(e){
     e.preventDefault();
    //获取dataTransfer对象的数据
     var title=e.dataTransfer.getData("title");
     var price=e.dataTransfer.getData("price");
     var goodsNames=cart.querySelectorAll(".goodsName");
     var hasSame;
     [ ].forEach.call(goodsNames,function(item,index,array){
      if(item.innerHTML===title){
       var innerHTML=item.previousSibling.innerHTML;
       item.previousSibling.innerHTML=parseInt(innerHTML)+1;
       hasSame=true;
      }
     });
     if(hasSame){
      return;
     }
     var cartWrap=document.createElement("div");
     cartWrap.className="cartWrap clrfix";
     var num=document.createElement("div");
     num.className="num";
     num.innerHTML=1;
     var goodsName=document.createElement("div");
     goodsName.className="goodsName";
     goodsName.innerHTML=title;
     var goodsPrice=document.createElement("div");
     goodsPrice.className="goodsPrice";
     goodsPrice.innerHTML=price;
     cartWrap.appendChild(num);
     cartWrap.appendChild(goodsName);
     cartWrap.appendChild(goodsPrice);
     cart.appendChild(cartWrap);
    })
   </script>
</body>
```
参考：
- 高三16章
- http://www.zhangxinxu.com/wordpress/2011/02/html5-drag-drop-%E6%8B%96%E6%8B%BD%E4%B8%8E%E6%8B%96%E6%94%BE%E7%AE%80%E4%BB%8B/