## 配置示例
```js
var option = {
          title: {
              text: 'ECharts 入门示例'
          },
          tooltip: {},
          legend: {
              data:['销量']
          },
          xAxis: {
              data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
          },
          yAxis: {},
          series: [{
              name: '销量',
              type: 'bar', //标识类型，bar为柱状图
              data: [5, 20, 36, 10, 10, 20]
          }]
      };
```
## 常见组件 (配置项)
### 1. 标题( title )
```js
var option={
    title:{
        show:true, //是否显示标题（默认true）
        text:'标题文字',
        subtext:'副标题',
        left:150, // 数字就是像素值，也可以是center/left/right
        borderColor:'red', // 标题边框颜色
        borderWidth:5 // 标题边框尺寸 
        //...
    }
}

```

### 2. 工具栏 ( toolbox )
```js
var option={
    toolbox:{
        show:true, //是否显示
        feature:{ //功能配置项
            dataView:{ //数据视图
                show:true
            }, 
            restore:{  //还原
                show:true
            }, 
            dataZoom:{ //区域缩放
                show:true
            },
            saveAsImage:{ //保存为图片
                show:true
            },
            magicType:{ //动态类型切换
                type:['line','bar']
            }
            //...
        }
        
    }
}
```

### 3. 弹窗 ( tooltip )
\- 鼠标悬停显示数据
```js
var option={
    tooltip:{
        trigger:'axis' //触发类型，'axis'表示坐标轴触发，'item'表示数据项图形触发(默认'item')
    }
}
```

### 4. 标记线和标记点 ( markLine 、 markPoint )
\- 最大值，平均值，最小值的标记线  
\- 任意位置的标记点
```js
var option={
    series:[{
        markPoint:{
            data:[
                {type:'max',name:'最大值'},
                {type:'min',name:'最小值'}
            ]
        },
        markLine:{
            data:[
                {type:'average',name:'平均值'} 
            ]
        }
    }]
    //...
}
```
## 饼状图
### 1. 配置示例
```js
var option = {
    backgroundColor: '#2c343c',

    title: {
        text: 'Customized Pie',
        left: 'center',
        top: 20,
        textStyle: {
            color: '#ccc'
        }
    },

    tooltip : {
        trigger: 'item',
        formatter: "{a} <br/>{b} : {c} ({d}%)"
    },

    visualMap: {
        show: false,
        min: 80,
        max: 600,
        inRange: {
            colorLightness: [0, 1]
        }
    },
    series : [
        {
            name:'访问来源',
            type:'pie',
            radius : '55%',
            center: ['50%', '50%'],
            data:[
                {value:335, name:'直接访问'},
                {value:310, name:'邮件营销'},
                {value:274, name:'联盟广告'},
                {value:235, name:'视频广告'},
                {value:400, name:'搜索引擎'}
            ].sort(function (a, b) { return a.value - b.value}),
            roseType: 'angle',
            label: {
                normal: {
                    textStyle: {
                        color: 'rgba(255, 255, 255, 0.3)'
                    }
                }
            },
            labelLine: {
                normal: {
                    lineStyle: {
                        color: 'rgba(255, 255, 255, 0.3)'
                    },
                    smooth: 0.2,
                    length: 10,
                    length2: 20
                }
            },
            itemStyle: {
                normal: {
                    color: '#c23531',
                    shadowBlur: 200,
                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
            }
        }
    ]
};
```
## 高级应用
