# 基础知识：
## 事件：
#### touchstart:  
\- 手指放到屏幕上的时候触发  （如果已经有手指放在屏幕上则不触发）
#### touchmove:   
\- 手指在屏幕上移动的时候触发
#### touchend:   
\- 手指从屏幕上拿起的时候触发
#### touchcancel:
\- 系统取消touch事件的时候触发。至于系统什么时候会取消，不详。。

## 事件属性：
#### event.touches
\- 跟踪触摸操作的touch对象数组（当前屏幕上所有触摸点的列表;**touchend事件中不含有该数组对象**）
#### event.targetTouches
\- 特定事件目标的touch对象数组（当前对象上所有触摸点的列表）
#### event.changeTouches  
\- 上次触摸改变的touch对象数组  
==注：jquery绑定touch事件时要使用原生事件对象event.originalEvent==
## event.touches对象属性：
- **clientX**：触摸目标在视口中的x坐标
- **clientY**：触摸目标在视口中的Y坐标
- **pageX**：触摸目标在页面中的x坐标（包括滚动）
- **pageY**：触摸目标在页面中的Y坐标（包括滚动）
- **screenX**：触摸目标在屏幕中的x坐标
- **screenY**：触摸目标在屏幕中的Y坐标
- **identifier**：标识触摸的唯一ID
- **target**：触摸的DOM节点目标
## BUG:
- **问题**：andriod只会触发一次touchstart，一次touchmove，touchend不触发
- **解决方案**：在touchmove中加入：event.preventDefault(),可fixedBug
- **注意**：event.preventDefault()会导致默认行为不发生，如scroll，导致页面不滚动

# 应用
- **局部滑动 scrollable类**

```js
/*局部滑动 scrollable类 */
$(document).on("touchstart", ".scrollable", function(e) {
    //this指向 .scrollable 元素
    var $this=$(this);
    var target=e.target;
    //记录手指按下的位置
    $this.data( "posY", e.originalEvent.touches[0].clientY ); /*e.originalEvent为原生event对象*/
    //记录起始滚动位置（距离）
    $this.data( "scrY", $(this).scrollTop() );
});
$(document).on("touchmove", ".scrollable", function(e) {
    e.preventDefault(); /*防止全屏滚动*/
    e.stopPropagation(); 
    var $this=$(this);
    //手指滑动距离
    var dis = e.originalEvent.touches[0].clientY - $this.data("posY");
    $this.scrollTop( $this.data("scrY") - dis );
});
```

- **jQuery注册tap事件**  

```js
/*jQuery注册tap事件*/

;(function ($) {
	var TOUCHSTART,TOUCHMOVE,TOUCHEND;
	//normal touch events
	if (typeof(window.ontouchstart) != 'undefined') {
	  TOUCHSTART = 'touchstart';
	  TOUCHMOVE = 'touchmove';
	  TOUCHEND   = 'touchend';
	//microsoft touch events(一般不考虑)
	} else if (typeof(window.onmspointerdown) != 'undefined') {
	  TOUCHSTART = 'MSPointerDown';
	  TOUCHMOVE = 'MSPointerMove';
	  TOUCHEND   = 'MSPointerUp';
	} else {
	  TOUCHSTART = 'mousedown';
	  TOUCHMOVE = 'mousemove';
	  TOUCHEND   = 'mouseup';
	}

	$.event.special.tap = {
	  setup: function() {
	    var elem = this;
	    $.event.add(elem, TOUCHSTART, function(event) {
        	$.event.trigger('tap', null, this);
	    });
	  },
	  teardown: function() {
	    var elem = this;
	    $.event.remove(elem, TOUCHSTART);
	  }
	};

	//插件模式，声明快捷方式：$(ele).tap(function () {});  
    $.fn.tap = function(callback) {
      return this.on('tap', callback);
    };

  })(jQuery);

```
- **swipe**  

```js
//swipe

//获取touch的点(兼容mouse事件)
var getTouchPos = function(e){
    var touches = e.touches;
    if(touches && touches[0]) {
        return { 
            x : touches[0].clientX ,
            y : touches[0].clientY 
        };
    }
    return { 
        x : e.clientX , 
        y : e.clientY 
    };
}

//计算两点之间距离
var getDist = function(p1 , p2){
    if(!p1 || !p2) return 0;
    return Math.sqrt((p1.x - p2.x) * (p1.x - p2.x) + (p1.y - p2.y) * (p1.y - p2.y));
}
//计算两点之间所成角度
var getAngle = function(p1 , p2){
    var r = Math.atan2(p2.y - p1.y, p2.x - p1.x);
    var a = r * 180 / Math.PI;
    return a;
};
 //获取swipe的方向
var getSwipeDirection = function(p2,p1){
    var dx = p2.x - p1.x;
    var dy = -p2.y + p1.y;    
    var angle = Math.atan2(dy , dx) * 180 / Math.PI;

    if(angle < 45 && angle > -45) return "right";
    if(angle >= 45 && angle < 135) return "top";
    if(angle >= 135 || angle < -135) return "left";
    if(angle >= -135 && angle <= -45) return "bottom";

}


var startEvtHandler = function(e){
	var pos = getTouchPos(e);
	ball.style.left = pos.x + 'px';
	ball.style.top = pos.y + 'px';
	ball.style.display = 'block';

	var touches = e.touches; 
    if( !touches || touches.length == 1 ){ //touches为空，代表鼠标点击
        point_start = getTouchPos(e);
        time_start = Date.now();
    }
}

var moveEvtHandler = function(e){
	var pos = getTouchPos(e);
	ball.style.left = pos.x + 'px';
	ball.style.top = pos.y + 'px';


	point_end = getTouchPos(e);
	e.preventDefault();
}

//touchend的touches和targetTouches没有对象，只有changeTouches才有
var endEvtHandler = function(e){
	ball.style.display = 'none';

	var time_end = Date.now();

	//距离和时间都符合
    if(getDist(point_start,point_end) > SWIPE_DISTANCE && time_start- time_end < SWIPE_TIME){
       
       var dir = getSwipeDirection(point_end,point_start);
       touchPad.innerHTML = 'swipe'+dir;
    }
}


var SWIPE_DISTANCE = 30;  //移动30px之后才认为swipe事件
var SWIPE_TIME = 500;     //swipe最大经历时间
var point_start,
	point_end,
	time_start,
	time_end;

//判断是PC或者移动设备
var startEvt, moveEvt, endEvt;
if("ontouchstart" in window){
    startEvt="touchstart";
    moveEvt="touchmove";
    endEvt="touchend";
}else{
    startEvt="mousedown";
    moveEvt="mousemove";
    endEvt="mouseup";            
}

touchpad.addEventListener(startEvt, startEvtHandler);
touchpad.addEventListener(moveEvt, moveEvtHandler);
touchpad.addEventListener(endEvt, endEvtHandler);
```

- **移动端双击放大图片**

```js
/*移动端双击放大图片*/
//判断是PC或者移动设备
    var startEvt, moveEvt, endEvt;
    if("ontouchstart" in window){
        startEvt="touchstart";
        moveEvt="touchmove";
        endEvt="touchend";
    }else{
        startEvt="mousedown";
        moveEvt="mousemove";
        endEvt="mouseup";            
    }

//判断双击的是否为图片
  function startHanlder(e){
       var target= e.target;
       if(target.nodeName.toLowerCase()==="img"){
           if( target.time===undefined ){
               target.time=new Date();
           }else{
               target.timeGap=new Date()-target.time;
               if(timeGap<500){
                  target.style.transform="scale(2)";
               }
           }
       }
}
```
