title:: js手写/Function/函数柯里化

- ## 定义
  collapsed:: true
	- 在数学和计算机科学中，柯里化是一种将使用多个参数的一个函数转换成一系列使用一个参数的函数的技术。
	- 举个例子：
	- ```js
	  function add(a, b) {
	      return a + b;
	  }
	  
	  // 执行 add 函数，一次传入两个参数即可
	  add(1, 2) // 3
	  
	  // 假设有一个 curry 函数可以做到柯里化
	  var addCurry = curry(add);
	  addCurry(1)(2) // 3
	  ```
## 第一版
collapsed:: true
	- 一个经常会看到的 curry 函数的实现为：
	- ```js
	  // 第一版
	  var curry = function (fn) {
	      var args = [].slice.call(arguments, 1);
	      return function() {
	          var newArgs = args.concat([].slice.call(arguments));
	          return fn.apply(this, newArgs);
	      };
	  };
	  ```
	- 可以这样使用：
	- ```js
	  function add(a, b) {
	      return a + b;
	  }
	  
	  var addCurry = curry(add, 1, 2);
	  addCurry() // 3
	  //或者
	  var addCurry = curry(add, 1);
	  addCurry(2) // 3
	  //或者
	  var addCurry = curry(add);
	  addCurry(1, 2) // 3
	  ```
	- 已经有柯里化的感觉了，但是还没有达到要求，不过我们可以把这个函数用作辅助函数，帮助我们写真正的 curry 函数。
- ## 第二版
  collapsed:: true
	- ```js
	  // 第二版
	  function sub_curry(fn) {
	      var args = [].slice.call(arguments, 1);
	      return function() {
	          return fn.apply(this, args.concat([].slice.call(arguments)));
	      };
	  }
	  
	  function curry(fn, length) {
	  
	      length = length || fn.length;
	  
	      var slice = Array.prototype.slice;
	  
	      return function() {
	          if (arguments.length < length) {
	              var combined = [fn].concat(slice.call(arguments));
	              return curry(sub_curry.apply(this, combined), length - arguments.length);
	          } else {
	              return fn.apply(this, arguments);
	          }
	      };
	  }
	  ```
	- 验证：
	- ```js
	  var fn = curry(function(a, b, c) {
	      return [a, b, c];
	  });
	  
	  fn("a", "b", "c") // ["a", "b", "c"]
	  fn("a", "b")("c") // ["a", "b", "c"]
	  fn("a")("b")("c") // ["a", "b", "c"]
	  fn("a")("b", "c") // ["a", "b", "c"]
	  ```
	- 效果已经达到我们的预期，然而这个 curry 函数的实现好难理解呐……
	- 为了让大家更好的理解这个 curry 函数，我给大家写个极简版的代码：
	- ```js
	  function sub_curry(fn){
	      return function(){
	          return fn()
	      }
	  }
	  
	  function curry(fn, length){
	      length = length || 4;
	      return function(){
	          if (length > 1) {
	              return curry(sub_curry(fn), --length)
	          }
	          else {
	              return fn()
	          }
	      }
	  }
	  
	  var fn0 = function(){
	      console.log(1)
	  }
	  
	  var fn1 = curry(fn0)
	  
	  fn1()()()() // 1
	  ```
	- 大家先从理解这个 curry 函数开始。
	- 当执行 fn1() 时，函数返回：
	- ```js
	  curry(sub_curry(fn0))
	  // 相当于
	  curry(function(){
	      return fn0()
	  })
	  ```
	- 当执行 fn1()() 时，函数返回：
	- ```js
	  curry(sub_curry(function(){
	      return fn0()
	  }))
	  // 相当于
	  curry(function(){
	      return (function(){
	          return fn0()
	      })()
	  })
	  // 相当于
	  curry(function(){
	      return fn0()
	  })
	  ```
	- 当执行 fn1()()() 时，函数返回：
	- ```js
	  // 跟 fn1()() 的分析过程一样
	  curry(function(){
	      return fn0()
	  })
	  ```
	- 当执行 fn1()()()() 时，因为此时 length > 2 为 false，所以执行 fn()：
	- ```js
	  fn()
	  // 相当于
	  (function(){
	      return fn0()
	  })()
	  // 相当于
	  fn0()
	  // 执行 fn0 函数，打印 1
	  ```
	- 再回到真正的 curry 函数，我们以下面的例子为例：
	- ```js
	  var fn0 = function(a, b, c, d) {
	      return [a, b, c, d];
	  }
	  
	  var fn1 = curry(fn0);
	  
	  fn1("a", "b")("c")("d")
	  ```
	- 当执行 fn1("a", "b") 时：
	- ```js
	  fn1("a", "b")
	  // 相当于
	  curry(fn0)("a", "b")
	  // 相当于
	  curry(sub_curry(fn0, "a", "b"))
	  // 相当于
	  // 注意 ... 只是一个示意，表示该函数执行时传入的参数会作为 fn0 后面的参数传入
	  curry(function(...){
	      return fn0("a", "b", ...)
	  })
	  ```
	- 当执行 fn1("a", "b")("c") 时，函数返回：
	- ```js
	  curry(sub_curry(function(...){
	      return fn0("a", "b", ...)
	  }), "c")
	  // 相当于
	  curry(function(...){
	      return (function(...) {return fn0("a", "b", ...)})("c")
	  })
	  // 相当于
	  curry(function(...){
	       return fn0("a", "b", "c", ...)
	  })
	  ```
	- 当执行 fn1("a", "b")("c")("d") 时，此时 arguments.length < length 为 false ，执行 fn(arguments)，相当于：
	- ```js
	  (function(...){
	      return fn0("a", "b", "c", ...)
	  })("d")
	  // 相当于
	  fn0("a", "b", "c", "d")
	  ```
	- 函数执行结束。
	- 所以，其实整段代码又很好理解：
	- sub_curry 的作用就是用函数包裹原函数，然后给原函数传入之前的参数，当执行 fn0(...)(...) 的时候，执行包裹函数，返回原函数，然后再调用 sub_curry 再包裹原函数，然后将新的参数混合旧的参数再传入原函数，直到函数参数的数目达到要求为止。
	- 如果要明白 curry 函数的运行原理，大家还是要动手写一遍，尝试着分析执行步骤。
## 更易懂的实现
collapsed:: true
	- 当然了，如果你觉得还是无法理解，你可以选择下面这种实现方式，可以实现同样的效果：
	- ```js
	  function curry(fn, args) {
	      var length = fn.length;
	  
	      args = args || [];
	  
	      return function() {
	  
	          var _args = args.slice(0),
	  
	              arg, i;
	  
	          for (i = 0; i < arguments.length; i++) {
	  
	              arg = arguments[i];
	  
	              _args.push(arg);
	  
	          }
	          if (_args.length < length) {
	              return curry.call(this, fn, _args);
	          }
	          else {
	              return fn.apply(this, _args);
	          }
	      }
	  }
	  
	  
	  var fn = curry(function(a, b, c) {
	      console.log([a, b, c]);
	  });
	  
	  fn("a", "b", "c") // ["a", "b", "c"]
	  fn("a", "b")("c") // ["a", "b", "c"]
	  fn("a")("b")("c") // ["a", "b", "c"]
	  fn("a")("b", "c") // ["a", "b", "c"]
	  ```
	- 或许大家觉得这种方式更好理解，又能实现一样的效果，为什么不直接就讲这种呢？
	- 因为想给大家介绍各种实现的方法嘛，不能因为难以理解就不给大家介绍呐~
- ## 第三版
  collapsed:: true
	- curry 函数写到这里其实已经很完善了，但是注意这个函数的传参顺序必须是从左到右，根据形参的顺序依次传入，如果我不想根据这个顺序传呢？
	- 我们可以创建一个占位符，比如这样：
	- ```js
	  var fn = curry(function(a, b, c) {
	      console.log([a, b, c]);
	  });
	  
	  fn("a", _, "c")("b") // ["a", "b", "c"]
	  ```
	- 我们直接看第三版的代码：
	- ```js
	  // 第三版
	  function curry(fn, args, holes) {
	      length = fn.length;
	  
	      args = args || [];
	  
	      holes = holes || [];
	  
	      return function() {
	  
	          var _args = args.slice(0),
	              _holes = holes.slice(0),
	              argsLen = args.length,
	              holesLen = holes.length,
	              arg, i, index = 0;
	  
	          for (i = 0; i < arguments.length; i++) {
	              arg = arguments[i];
	              // 处理类似 fn(1, _, _, 4)(_, 3) 这种情况，index 需要指向 holes 正确的下标
	              if (arg === _ && holesLen) {
	                  index++
	                  if (index > holesLen) {
	                      _args.push(arg);
	                      _holes.push(argsLen - 1 + index - holesLen)
	                  }
	              }
	              // 处理类似 fn(1)(_) 这种情况
	              else if (arg === _) {
	                  _args.push(arg);
	                  _holes.push(argsLen + i);
	              }
	              // 处理类似 fn(_, 2)(1) 这种情况
	              else if (holesLen) {
	                  // fn(_, 2)(_, 3)
	                  if (index >= holesLen) {
	                      _args.push(arg);
	                  }
	                  // fn(_, 2)(1) 用参数 1 替换占位符
	                  else {
	                      _args.splice(_holes[index], 1, arg);
	                      _holes.splice(index, 1)
	                  }
	              }
	              else {
	                  _args.push(arg);
	              }
	  
	          }
	          if (_holes.length || _args.length < length) {
	              return curry.call(this, fn, _args, _holes);
	          }
	          else {
	              return fn.apply(this, _args);
	          }
	      }
	  }
	  
	  var _ = {};
	  
	  var fn = curry(function(a, b, c, d, e) {
	      console.log([a, b, c, d, e]);
	  });
	  
	  // 验证 输出全部都是 [1, 2, 3, 4, 5]
	  fn(1, 2, 3, 4, 5);
	  fn(_, 2, 3, 4, 5)(1);
	  fn(1, _, 3, 4, 5)(2);
	  fn(1, _, 3)(_, 4)(2)(5);
	  fn(1, _, _, 4)(_, 3)(2)(5);
	  fn(_, 2)(_, _, 4)(1)(3)(5)
	  ```
- ## 自己实现版
  collapsed:: true
	- ```js
	  function curry(fn, args = []) {
	    var length = fn.length
	  
	    return function () {
	      for (var i = 0; i < arguments.length; i++) {
	        args.push(arguments[i])
	      }
	      if (args.length < length) {
	        return curry.call(this, fn, args)
	      } else {
	        return fn.apply(this, args)
	      }
	    }
	  }
	  
	  var fn = curry(function (a, b, c) {
	    return [a, b, c]
	  })
	  
	  var res = fn(1)(2)(3)
	  console.log('res', res)
	  
	  ```