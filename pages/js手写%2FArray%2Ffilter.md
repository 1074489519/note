title:: js手写/Array/filter

- 参数
	- `item`：遍历项
	- `index`：遍历项的索引
	- `arr`：数组本身
- 实现
	- ```js
	  Array.prototype.myFilter = function (callback) {
	    const res = []
	    for (let i = 0; i < this.length; i++) {
	      callback(this[i], i, this) && res.push(this[i])
	    }
	    return res
	  }
	  ```