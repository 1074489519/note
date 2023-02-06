title:: js手写/Object/is

- 用处
	- `Object.is(a, b)`，判断a是否等于b
- 实现
	- ```js
	  Object.prototype.myIs = function (x, y) {
	    if (x === y) {
	      // 防止 -0 和 +0
	      return x !== 0 || 1 / x === 1 / y
	    }
	  
	    // 防止NaN
	    return x !== x && y !== y
	  }
	  ```