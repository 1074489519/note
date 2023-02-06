title:: js手写/Function/compose函数

- 简单的`compose`函数
  title:: js手写/Function/compose函数
	- ```js
	  const compose = (a, b) => c => a(b(c))
	  ```
- 案例
	- ```js
	  const space = (str) => str.split(' ')
	  const len = (arr) => arr.length
	  
	  // 普通写法
	  console.log(len(space('i am jack'))) // 3
	  console.log(len(space('i am 23 year old'))) // 5
	  
	  // compose写法
	  const compose = (...fns) => value => {
	    return fns.reduce((value, fn) => {
	      return fn(value)
	    }, value)
	  }
	  const computedWord = compose(space, len)
	  console.log(computedWord('i am jack')) // 3
	  console.log(computedWord('i am 23 year old')) // 5
	  ```