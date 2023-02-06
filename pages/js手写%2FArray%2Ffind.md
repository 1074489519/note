title:: js手写/Array/find

- 参数
	- `item`：遍历项
	- `index`：遍历项的索引
	- `arr`：数组本身
- 实现
	- ```js
	  Array.prototype.myFind = function (callback) {
	    for (let i = 0; i < this.length; i++) {
	      if (callback(this[i], i, this)) {
	        return this[i]
	      }
	    }
	    return undefined
	  }
	  ```