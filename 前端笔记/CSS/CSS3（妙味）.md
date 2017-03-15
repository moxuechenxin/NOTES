## 新的UI方案
- ### 圆角
    - #### border-radius: 1-4个数字 / 1-4个数字
        - ##### 前面是水平，后面是垂直  
        - ##### 不给“/”则水平和垂直一样  
        - ##### border-radius: 10px/5px;
        
    - #### 参数
        - ##### 前各种长度单位都可以：px，%，…
        - ##### %有时很方便
            \- 但宽高不一致时不太好
- ### 边框
    - #### 边框图片 border-image
        - ##### border-image-sourceg 引入图片
        - ##### border-image-slice 切割图片
        - ##### border-image-width 边框宽度
        - ##### border-image-repeat 图片的排列方式
            \- round 平铺,repeat 重复,stretch拉伸 

   - #### 边框颜色 border-colors
- ### 线性渐变
    - #### linear-gradient([<起点> || <角度>,]? <点>, <点>…)
    - #### 只能用在背景上
        ```
        IE filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#ffffff',endColorstr='#ff0000',GradientType='1');
        ```
    - #### 参数
        \- 起点：从什么方向开始渐变  left、top、left top  
        \- 角度：从什么角度开始渐变  xxx deg的形式  
        \- 点：渐变点的颜色和位置  black 50%，位置可选  







 