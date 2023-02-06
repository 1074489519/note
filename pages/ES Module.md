- ## 概述
	- > ES6模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输入的变量。
	  [[CommonJS]]和[[AMD]]模块，都只能在运行时确定这些东西。比如：[[CommonJS]]模块就是对象，输入时必须查找对象属性。
- ## 语法
	- 定义暴露模块（export）：
		- ```js
		  // 定义模块 math.js
		  var basicNum = 0
		  var add = functinon(a, b) {
		    return a + b
		  }
		  export { basicNum, add }
		  ```
		- 使用`import`命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。为了给用户提供方便，让他们不用阅读文档就能加载模块，就要用到`export default`命令，为模块指定默认输出。
		- ```js
		  // export-default.js
		  export default function() {
		    console.log('foo')
		  }
		  ```
	- 引用使用模块（import）：
		- ```js
		  // 应用模块
		  import { basicNum, add } from './math'
		  function test(ele) {
		    ele.textContent = add(99 + basicNum)
		  }
		  // import-default.js
		  import customName from './export-default'
		  customName() // foo
		  ```
		- 模块默认输出, 其他模块加载该模块时，import命令可以为该匿名函数指定任意名字。
-