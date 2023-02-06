title:: js手写/Object/fromEntries

- 用处
	- 跟 [[js手写/Object/entries]] 相反，将键值对数组转成对象
- 实现
	- ```js
	  Object.prototype.myFromEntries = function (arr) {
	    const res = {}
	    for (let i = 0; i < arr.length; i++) {
	      const [key, val] = arr[i]
	      res[key] = val
	    }
	    return res
	  }
	  ```