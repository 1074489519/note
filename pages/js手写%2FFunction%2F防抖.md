title:: js手写/Function/防抖

- ## 概念
	- 触发多次事件，一定在事件触发 n 秒后才执行，如果在一个事件触发的 n 秒内又触发了这个事件，那我就以新的事件的时间为准，n 秒后才执行。
- ## 实现
	- ```js
	  function debounce(func, wait = 150) {
	    var timerout, context, args
	    return function () {
	      context = this
	      args = arguments
	      
	      clearTimeout(timerout)
	      timerout = setTimeout(function () {
	        func.apply(context, args)
	      }, wait)
	    }
	  }
	  ```