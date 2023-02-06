title:: js手写/String/slice

- 参数
	- `start`：开始截取的字符索引（包含此字符）
	- `end`：结束截取的字符索引（不包含此字符）
	- `start > end`：返回空字符串
	- `start < 0`：start = 数组长度 + start
- 实现
	- ```js
	  String.prototype.mySlice = function (start = 0, end) {
	    start = start < 0 ? this.length + start : start
	    end = !end && end !== 0 ? this.length : end
	  
	    if (start >= end) return ""
	    let str = ""
	    for (let i = start; i < end; i++) {
	      str += this[i]
	    }
	    return str
	  }
	  ```