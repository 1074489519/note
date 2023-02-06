title:: js手写/Promise/race

- title:: js手写/Promise/race
- 概述
	- `Promise.race(iterable)` 方法返回一个 promise，一旦迭代器中的某个 promise 解决或拒绝，返回的 promise 就会解决或拒绝。
- 实现
	- ```js
	  Promise.MyRace = function (promises) {
	    return new Promise((resolve, reject) => {
	      // 这里不需要使用索引，只要能循环出每一项就行
	      for (const item of promises) {
	        Promise.resolve(item).then(resolve, reject)
	      }
	    })
	  }
	  ```