title:: js手写/Array/includes

	- 用处
	  collapsed:: true
		- 查找元素，查到返回 `true` ，反之返回 `false`
		- 可查找 `NaN`
	- 实现
	  collapsed:: true
		- ```js
		  Array.prototype.myIncludes = function (value, start = 0) {
		    if (start < 0) start = this.length + start
		    const isNaN = Number.isNaN(value)
		  
		    for (let i = start; i < this.length; i++) {
		      if (this[i] === value || (isNaN && Number.isNaN(this[i]))) {
		        return true
		      }
		    }
		    return false
		  }
		  ```
-