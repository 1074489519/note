title:: js手写/String/substr

- 参数
	- `start`：开始截取的字符索引(包含此字符)
	- `length`：截取的长度 注意点
	- `start < 0`： `start = 数组长度 + start`
	- `length`超出所能截取范围，需要做处理
	- `length < 0`：返回空字符串
- 实现
	- ```js
	  String.prototype.mySubStr = function (start = 0, length) {
	    if (length < 0) return ""
	    start = start < 0 ? this.length + start : start
	    length =
	      (!length && length !== 0) || length > this.length - start
	        ? this.length
	        : start + length
	  
	    let str = ""
	    for (let i = start; i < length; i++) {
	      str += this[i]
	    }
	    return str
	  }
	  ```