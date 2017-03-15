
```
//自定义事件
;(function ($) {
	var TOUCHSTART, TOUCHEND;
	//normal touch events
	if (typeof(window.ontouchstart) != 'undefined') {
	  TOUCHSTART = 'touchstart';
	  TOUCHMOVE = 'touchmove';
	  TOUCHEND   = 'touchend';
	//microsoft touch events
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
	// 插件模式，声明快捷方式：$(elem).tap(function () {});
    $.fn.tap = function(callback) {
      return this.on('tap', callback);
    };
  })(jQuery);
```
