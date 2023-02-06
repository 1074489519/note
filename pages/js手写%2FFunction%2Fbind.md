title:: js手写/Function/bind

- 简介
	- > bind() 方法会创建一个新函数。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，之后的一序列参数将会在传递的实参前传入作为它的参数。
	- 由此我们可以首先得出 bind 函数的两个特点： #.ol
		- 返回一个函数
		- 可以传入参数
- 用法
	- ```js
	  Function.bind(thisArg[, arg1[, arg2[, ...]]])
	  ```
	- bind 方法 与 apply 和 call 比较类似，也能改变函数体内的 this 指向。不同的是，**bind 方法的返回值是函数，并且需要稍后调用，才会执行**。而 apply 和 call 则是立即调用。
	- ```js
	  function add (a, b) {
	      return a + b;
	  }
	  
	  function sub (a, b) {
	      return a - b;
	  }
	  
	  add.bind(sub, 5, 3); // 这时，并不会返回 8
	  add.bind(sub, 5, 3)(); // 调用后，返回 8
	  ```
	- 如果 bind 的第一个参数是 null 或者 undefined，this 就指向全局对象 window。
### 实现
	- Function.prototype.bind2 = function (context) {
	  
	      if (typeof this !== "function") {
	        throw new Error("Function.prototype.bind - what is trying to be bound is not callable");
	      }
	  
	      var self = this;
	      var args = Array.prototype.slice.call(arguments, 1);
	  
	      var fNOP = function () {};
	  
	      var fBound = function () {
	          var bindArgs = Array.prototype.slice.call(arguments);
	          return self.apply(this instanceof fNOP ? this : context, args.concat(bindArgs));
	      }
	  
	      fNOP.prototype = this.prototype;
	      fBound.prototype = new fNOP();
	      return fBound;
	  }
	- 参考：[https://github.com/mqyqingfeng/Blog/issues/12](https://github.com/mqyqingfeng/Blog/issues/12)
	- 验证
	- ```js
	  var value = 2;
	  
	  var foo = {
	      value: 1
	  };
	  
	  function bar(name, age) {
	      this.habit = 'shopping';
	      console.log(this.value);
	      console.log(name);
	      console.log(age);
	  }
	  
	  bar.prototype.friend = 'kevin';
	  
	  var bindFoo = bar.bind(foo, 'daisy');
	  
	  var obj = new bindFoo('18');
	  // undefined
	  // daisy
	  // 18
	  console.log(obj.habit);
	  console.log(obj.friend);
	  // shopping
	  // kevin
	  ```