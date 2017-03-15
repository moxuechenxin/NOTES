
```
/*样式*/
    /*包裹滑动内容的元素*/
.mscroll-box {
    position: relative;
    overflow: hidden;
}
    /*滑动内容*/
.mscroll-body{
	position: absolute;
	top:0;
	left:15px;
	right:15px;
}
    /*滑动条*/
.mscroll-bar{
	position: absolute;
	width: 10px;
	height: 100%;
	background: #fff;
	border-left: 1px solid #ccc;
	right: 0;
	top: 0;
}
    /*滑动块*/
.mscroll-block{
	position: absolute;
	width: 8px;
	height: 20px;
	top: 0;
	left: 0;
	margin: 1px;
	box-sizing: border-box;
	border: 1px solid #ccc;
}

/*js*/
  //jQuery插件模式
		;$.fn.extend({
			mscroll:function(opts){
				opts=$.extend({
				},opts);
				this.each(function(index,el){
					var $that=$(el);
					$that.addClass('mscroll-box');
					var body=$that.wrapInner('<div class="mscroll-body"></div>').children('.mscroll-body');
					var bar=$('<div class="mscroll-bar"><div class="mscroll-block"></div></div>');

					$that.append(bar);
					var block=$that.find(".mscroll-block");
					//滑块可滑动距离
					var block_h=block.outerHeight(true),
					bar_h=$that.find(".mscroll-bar").height(),
					block_y_length=bar_h - block_h;
					//内容可滚动距离
					var box_h=$that.height(),
					body_h=body.outerHeight(true),
					body_y_length=body_h - box_h;

					//滑块控制
					var scrollbar_down;
					block.on("mousedown",function(e){
						scrollbar_down=true;
						block.data("originTop",$(this).position().top);
						block.data("offsetY",e.pageY);
					});
					$(document).on("mouseup",function(){
						scrollbar_down=false;
					});
					$(document).on("mousemove",function(e){
						if(scrollbar_down){
							var scrollbar_block_dom=block[0];
							var top=block.data("originTop")+e.pageY-block.data("offsetY");
							top=top<=0?0:top>=block_y_length?block_y_length:top;
							scrollbar_block_dom.style.top=top+"px";
							body[0].style.top=- top / block_y_length * body_y_length + "px";
						}
					});

					//滚轮控制
					body.on("mousewheel DOMMouseScroll",function(e){
						e.preventDefault();
						var _$this=$(this);
						var oEvent=e.originalEvent;
						var delta = (oEvent.wheelDelta && oEvent.wheelDelta / 120 ) /*chrome & ie*/ || 
						            (oEvent.detail && oEvent.detail / -3 ); /*firefox*/
						//向上为正，向下为负
						var wheelstep=50;
						var top=_$this.position().top+delta*wheelstep;
						top=top>=0?0:top<-body_y_length?(-body_y_length):top;
						body[0].style.top=top+"px";
						block[0].style.top=- top / body_y_length * block_y_length + "px";
					});

				});
			}
		});
```
