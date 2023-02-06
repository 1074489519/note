title:: js手写/Object/keys

- 用处
	- 将对象的key转成一个数组合集
- 实现
	- ```js
	  Object.prototype.myKeys = function (obj) {
	    const res = []
	    for (let key in obj) {
	      if (obj.hasOwnProperty(key)) {
	        res.push(key)
	      }
	    }
	    return res
	  }
	  ```