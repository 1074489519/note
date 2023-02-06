title:: js手写/Object/entries

- 用处
	- 将对象转成键值对数组
- 实现
	- ```js
	  Object.prototype.myEntries = function (obj) {
	    const res = []
	    for (let key in obj) {
	      if (obj.hasOwnProperty(key)) {
	        res.push([key, obj[key]]) 
	      }
	    }
	    return res
	  }
	  ```