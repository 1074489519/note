title:: js手写/Array/some

- 参数
	- `item`：遍历项
	- `index`：遍历项的索引
	- `arr`：数组本身
- 实现
	- ```js
	  Array.prototype.mySome = function (callback) {
	    for (let i = 0; i < this.length; i++) {
	      if (callback(this[i], i, this)) {
	        return true
	      }
	    }
	    return false
	  }
	  ```