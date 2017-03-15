- ## 新版弹性盒子
    - 兼容性：IE10+，chrome21+，opera12.1+，Firefox22+，safari6.1+等
    - ### 父元素属性：
        - #### display:-webkit-flex; display:-webkit-inline-flex; (行内元素,有包裹性)
            - ##### 旧版：display:-webkit-box;
            注意，设为Flex布局以后，子元素的float、clear和vertical-align属性将失效
        - #### flex-direction：
            \- 决定主轴的方向（即项目的排列方向）
            - ##### 可选值：
                - ###### row（默认值）：主轴为水平方向，起点在左端。
                - ###### row-reverse：主轴为水平方向，起点在右端。
                - ###### column：主轴为垂直方向，起点在上沿。
                - ###### column-reverse：主轴为垂直方向，起点在下沿。
            - ##### ==旧版==：该属性实现了旧版中box-orient和box-direction的合并功能
        - #### flex-wrap：
            \- 属性定义，如果一条轴线（一行或一列）排不下，如何换行
            - ##### 可选值：
                - ###### nowrap（默认）：不换行。
                - ###### wrap：换行，第一行在上方
                - ###### wrap-reverse：换行，第一行在下方
            - ##### ==旧版==：
        - #### flex-flow：
            \- 是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap
            - ##### ==旧版==：
        - #### justify-content：
            \- 定义项目在主轴上的对齐方式
            - ##### 可选值：具体对齐方式与轴的方向有关。下面假设主轴为从左到右
                - ###### flex-start（默认值）：左对齐
                - ###### flex-end：右对齐
                - ###### center： 居中
                - ###### space-between：两端对齐，项目之间的间隔都相等。
                - ###### space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
            - ##### ==旧版==：类似box-pack
        - #### align-items：
            \- 定义项目在交叉轴上如何对齐
            - ##### 可选值：具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下
                - ###### flex-start：交叉轴的起点对齐。
                - ###### flex-end：交叉轴的终点对齐。
                - ###### center：交叉轴的中点对齐。
                - ###### baseline: 项目的第一行文字的基线对齐。
                - ###### stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
            - ##### ==旧版==：类似box-align
        - #### align-content：
            \- 定义多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用
            - ##### 可选值：
                - ###### flex-start：与交叉轴的起点对齐
                - ###### flex-end：与交叉轴的终点对齐
                - ###### center：与交叉轴的中点对齐
                - ###### space-between：与交叉轴两端对齐，轴线之间的间隔平均分布
                - ###### space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍
                - ###### stretch（默认值）：轴线占满整个交叉轴
            - ##### ==旧版==：
    - ### 子元素属性：
        - #### order：
            \- 定义项目的排列顺序。数值越小，排列越靠前，默认为0
            - ##### ==旧版==：
        - #### flex-grow：
            \- 定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大
            - ##### 参数说明：  
                \- 如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。  
                \- 如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍
            - ##### ==旧版==：
        - #### flex-shrink：
            \- 定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
            - ##### 参数说明：
                \- 如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小  
                \- 如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小
                \- 负值对该属性无效
            - ##### ==旧版==：
        - #### flex-basis：
            \- 定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小
            - ##### 可选值：
                - ###### auto (默认值)
                - ###### <length> 
            - ##### 参数说明：
                \- 可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间
            - ##### ==旧版==：
        - #### flex：
            \- 是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选
            - ##### 参数说明：
                \- 该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)  
                \- 后两个属性可选  
                \- 建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关  
            - ##### ==旧版==：-webkit-box-flex
        - #### align-self：
            \- 允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性
            - ##### 参数说明：
                \- 默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch  
                \- 该属性可能取6个值，除了auto，其他都与align-items属性完全一致  

- ## 旧版弹性盒子
    - ### 父元素属性：
        - #### display:-webkit-box; display:-webkit-inline-box;
            - ##### ==新版==：display:-webkit-flex;
        - #### box-orient：
            \- 确定子元素的方向。是横着排还是竖着走
            - ##### 可选值：
                - ###### inline-axis：元素横排 (默认值)  
                - ###### horizontal：元素横排  
                - ###### vertical：元素纵列  
                - ###### block-axis：元素纵列  
                - ###### inherit
            - ##### ==新版==：类似flex-direction
        - #### box-direction：
            \- 确定子元素的排列顺序
            - ##### 可选值：
                - ###### normal：从左到右或从上到下 (默认值)  
                - ###### reverse 顺序反转
                - ###### inherit
            - ##### ==新版==： 类似flex-direction
        - #### box-pack：
            \- box-align与box-pack都是决定盒子内部剩余空间怎么使用的，在行为效果上就是表现为“对齐”。box-pack决定了父标签水平遗留空间的使用，即水平方向上的对齐
            - ##### 可选值：
                - ###### start (默认值)：与父标签的起始位置对齐  
                - ###### end：与父标签的结束位置对齐  
                - ###### center：水平居中对齐  
                - ###### justify：两端对齐
            - ##### ==新版==：类似justify-content
        - #### box-align：
            \- box-align与box-pack都是决定盒子内部剩余空间怎么使用的，在行为效果上就是表现为“对齐”。box-align决定了垂直方向上的空间利用，也就是垂直方向上的对齐表现
            - ##### 可选值：
                - ###### stretch (默认值)：拉伸，也就是父标签高度多高，其孩子元素的高度就多高  
                - ###### start：起始边对齐  
                - ###### end：结束边对齐  
                - ###### center：居中对齐  
                - ###### baseline：基线对齐  
            - ##### ==新版==：类似align-items
        - #### box-lines：（所有浏览器都不支持）
            \- 用来决定子元素一行排不下，是否可以换行显示
            - ##### 可选值：          
                - ###### single (默认值)：不换行  
                - ###### multiple :如果排不下则换行   
    - ### 子元素属性：
        - #### -webkit-box-flex
            - ##### 可选值：
                - ###### <number>
            - ##### ==新版==：-webkit-flex
参考：  
https://zhengjitf.github.io/hello_mobile/web_flex.html  
http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html
http://www.zhangxinxu.com/wordpress/2010/12/css-box-flex%E5%B1%9E%E6%80%A7%EF%BC%8C%E7%84%B6%E5%90%8E%E5%BC%B9%E6%80%A7%E7%9B%92%E5%AD%90%E6%A8%A1%E5%9E%8B%E7%AE%80%E4%BB%8B/ (旧版)  
http://www.runoob.com/css3/css3-flexbox.html



