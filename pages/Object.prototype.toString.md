title:: Object.prototype.toString

- 当 toString 方法被调用的时候，下面的步骤会被执行： #.ol
	- 如果 this 值是 undefined，就返回 [object Undefined]
	- 如果 this 的值是 null，就返回 [object Null]
	- 让 O 成为 ToObject(this) 的结果
	- 让 class 成为 O 的内部属性 \[[Class]] 的值
	- 最后返回由 "[object " 和 class 和 "]" 三个部分组成的字符串
- Object.prototype.string至少能识别12种类型：
	- ```js
	  // 以下是11种：
	  var number = 1;          // [object Number]
	  var string = '123';      // [object String]
	  var boolean = true;      // [object Boolean]
	  var und = undefined;     // [object Undefined]
	  var nul = null;          // [object Null]
	  var obj = {a: 1}         // [object Object]
	  var array = [1, 2, 3];   // [object Array]
	  var date = new Date();   // [object Date]
	  var error = new Error(); // [object Error]
	  var reg = /a/g;          // [object RegExp]
	  var func = function a(){}; // [object Function]
	  
	  // 除了以上 11 种之外，还有：
	  console.log(Object.prototype.toString.call(Math)); // [object Math]
	  console.log(Object.prototype.toString.call(JSON)); // [object JSON]
	  
	  // 除了以上 13 种之外，还有：
	  function fn() {
	      console.log(Object.prototype.toString.call(arguments)); // [object Arguments]
	  }
	  fn();
	  ```