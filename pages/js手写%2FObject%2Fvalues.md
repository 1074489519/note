title:: js手写/Object/values

- 用处
	- 将对象的所有值转成数组合集
- 实现
	- ```js
	  Object.prototype.myValues = function (obj) {
	    const res = []
	    for (let key in obj) {
	      if (obj.hasOwnProperty(key)) {
	        res.push(obj[key])
	      }
	    }
	    return res
	  }
	  ```