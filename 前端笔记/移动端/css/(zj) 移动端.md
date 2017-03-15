- ###  viewport
    - #### 两个概念
        - ##### layout viewport
            \- 一般默认980px (ios)  
            \- 通过document.body.clientWidth获取
        - ##### visual viewport
            \- 一般屏幕宽度/dpr  
            \- 通过window.innerWidth获取  
            \- 页面缩放比可通过window.innerWidth / document.body.clientWidth获取
    - #### meta标签
        
        ```
        //格式
        <meta name="viewport" content="name=value,name=value">
        //参数
        /*
         *width：设置layout viewport的特定值("device-width")
         *initial-scale：设置页面的初始缩放
         *minimum-scale：最小缩放
         *maximum-scale：最大缩放
         *user-scalable：用户能否缩放
         *
         */
         
         //手机端常用viewport设置
         <meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no">
        ```
        常用meta：https://segmentfault.com/a/1190000002407912  
        http://www.runoob.com/w3cnote/html-meta-intro.html