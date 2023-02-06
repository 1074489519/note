- ## 概述：
	- CMD规范专门用于浏览器端，模块的加载是异步的，模块使用时才会加载执行。CMD规范整合了CommonJS和AMD规范的特点。在Sea.js中，所有JavaScript模块都遵循CMD模块定义规范。
- ## 语法
	- 定义暴露模块：
		- ```js
		  // 定义没有依赖的模块
		  define(function(require, exports, module) {
		    exports.xxx = value
		    module.exports = value
		  })
		  ```
		- ```js
		  // 定义有依赖的模块
		  define(function(require, exports, module) {
		    // 引入依赖模块（同步）
		    var module2 = require('./module2')
		    // 引入依赖模块（异步）
		    require.async('./module3', function(m3) {
		      //...
		    })
		    // 暴露模块
		    exports.xxx = value
		  })
		  ```
	- 引入使用模块：
		- ```js
		  define(functnion(require) {
		     	var m1 = require('./module1')
		  	var m2 = require('./module4')
		      m1.show()
		  	m4.show()
		  })
		  ```