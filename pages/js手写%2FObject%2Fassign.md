title:: js手写/Object/assign

- 难点
	- assign接收多个对象，并将多个对象合成一个对象
	- 这些对象如果有重名属性，以后来的对象属性值为准
	- assign返回一个对象， `这个对象 === 第一个对象`
- 实现
	- ```js
	  Object.prototype.myAssign = function (target, ...args) {
	    if (target === null || target === undefined) {
	      throw new TypeError("Connot convert undefined or null to object")
	    }
	    target = Object(target)
	    for (let nextObj of args) {
	      for (let key in nextObj) {
	        if (nextObj.hasOwnProperty(key)) {
	          target[key] = nextObj[key]
	        }
	      }
	    }
	    return target
	  }
	  ```