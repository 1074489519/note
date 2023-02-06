title:: js手写/Object/instanceOf

- 用处
	- `A instanceOf B`，判断A是否经过B的原型链
- 实现
	- ```js
	  function instanceOf(left, right) {
	    // 获取对象的原型
	    const proto = Object.getPrototypeOf(left)
	    // 获取构造函数的 prototype 对象
	    let prototyoe = right.prototype
	  
	    // 判断构造函数的 prototype 对象是否在对象的原型上
	    while (true) {
	      if(!proto) return false;
	      if(proto === prototype) return true 
	      // 如果没有找到，就继续从其与原型上找， Object.getPrototypeOf方法用来获取指定对象的原型
	      proto = Object.getPrototypeOf(proto)
	    }
	  }
	  ```