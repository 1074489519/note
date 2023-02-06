title:: js手写/Promise/allSettled

- 概述
	- 该 `Promise.allSettled()` 方法返回一个在所有给定的 `promise` 都已经 `fulfilled` 或 `rejected` 后的 promise，并带有一个对象数组，每个对象表示对应的 promise 结果。
- 实现
	- ```js
	  Promise.MyAllSettled = function (promises) {
	    let arr = [],
	      count = 0
	    return new Promise((resolve, reject) => {
	      const processResult = (res, index, status) => {
	        arr[index] = { status: status, val: res }
	        count += 1
	        if (count === promises.length) resolve(arr)
	      }
	  
	      promises.forEach((item, i) => {
	        Promise.resolve(item).then(res => {
	          processResult(res, i, 'fulfilled')
	        }, err => {
	          processResult(err, i, 'rejected')
	        })
	      })
	    })
	  }
	  ```