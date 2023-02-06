title:: js手写/Array/fill

- 参数
	- `initValue`：填充的值
	- `start`：开始填充索引，默认`0`
	- `end`：结束填充索引，默认`length`
- 实现
	- ```js
	  Array.prototype.myFill = function (value, start = 0, end = this.length) {
	    for (let i = start; i < end; i++) {
	      this[i] = value
	    }
	  }
	  ```