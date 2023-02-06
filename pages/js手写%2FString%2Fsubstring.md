title:: js手写/String/substring

- 功能与 `slice` 大致相同
- 区别之处
	- start > end：互换值
- 实现
	- ```js
	  String.prototype.mySubstring = function (start = 0, end) {
	    start = start < 0 ? this.length + start : start
	    end = !end && end !== 0 ? this.length : end
	  
	    if (start >= end) [start, end] = [end, start]
	  
	    let str = ""
	    for (let i = start; i < end; i++) {
	      str += this[i]
	    }
	    return str
	  }
	  ```