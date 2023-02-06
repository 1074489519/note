title:: js手写/Function/call

- 用法
	- ```js
	  Function.call(obj,[param1[,param2[,…[,paramN]]]])
	  ```
	- 需要注意以下几点： #.ol
		- 调用 call 的对象，必须是个函数 Function。
		- call 的第一个参数，是一个对象。 Function 的调用者，将会指向这个对象。如果不传，则默认为全局对象 window。
		- 第二个参数开始，可以接收任意个参数。每个参数会映射到相应位置的 Function 的参数上。但是如果将所有的参数作为数组传入，它们会作为一个整体映射到 Function 对应的第一个参数上，之后参数都为空。
	- ```js
	  function func (a,b,c) {}
	  func.call(obj, 1,2,3)
	  // func 接收到的参数实际上是 1,2,3
	  func.call(obj, [1,2,3])
	  // func 接收到的参数实际上是 [1,2,3],undefined,undefined
	  ```
- 实现
	- ```js
	  Function.prototype.myCall = function (context) {
	    context = context ? Object(context) : window
	    context.fn = this // 重置上下文
	    var args = [...arguments].slice(1) // 截取参数a,b
	    var r = context.fn(...args) // 执行函数
	    delete context._fn // 删除属性，避免污染
	    return r // 返回结果
	  }
	  ```
	- 验证
	- ```js
	  var a = 1,
	    b = 2
	  var obj = { a: 10, b: 20 }
	  function test(key1, key2) {
	    console.log(this[key1], this[key2])
	  }
	  test("a", "b") 	// 1 2 
	  test.myCall(obj, "a", "b") // 10 20
	  ```