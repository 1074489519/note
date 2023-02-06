title:: js手写/Array/reduce

- 参数
	- `prev`：前一项
	- `next`：下一项
	- `index`：当前索引
	- `arr`：数组本身
- 实现
	- ```js
	  Array.prototype.myReduce = function (callback, ...args) {
	    let start = 0,
	      prev
	  
	    if (args.length) {
	      prev = args[0]
	    } else {
	      prev = this[0]
	      start = 1
	    }
	    for (let i = start; i < this.length; i++) {
	      prev = callback(prev, this[i], i, this)
	    }
	    return prev
	  }
	  ```