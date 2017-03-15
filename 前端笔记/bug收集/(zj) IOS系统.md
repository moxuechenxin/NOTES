### 1. iframe不支持滚动
**机型及系统**：iphone 5s -- ios 9.3.5  
**解决**：
    
    ```
    //html 
    <div class="scroll-wrapper">  
        <iframe src=""></iframe>  
    </div>  
    
    //css
    .scroll-wrapper {  
        -webkit-overflow-scrolling: touch;  
        overflow-y: scroll;  
        /* 提示: 请在此处加上需要设置的大小(dimensions)或位置(positioning)信息! */  
    }  
    .scroll-wrapper iframe {  
        height: 100%;  
        width: 100%; 
        /* 你自己指定的样式 */  
    }  
    ```
