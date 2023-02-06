title:: js手写/Array/forEach

- 参数
	- `item`：遍历项
	- `index`：遍历项的索引
	- `arr`：数组本身
- 实现
	- ```js
	  Array.prototype.myForEach = function (callback) {
	    for (let i = 0; i < this.length; i++) {
	      callback(this[i], i, this)
	    }
	  }
	  ```
-