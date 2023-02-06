title:: js手写/Promise/all

- 概述
	- `Promise.all` 的返回值是一个新的 `Promise` 实例
	- `Promise.all` 接受一个可遍历的数据容器，容器中每个元素都应是 `Promise` 实例，容器就是数组。
	- 数组中的每个 `Promise` 实例都成功时，`Promise.all`才成功。这些 `Promise`实例所有的`resolve`结果会按照原来的顺序集合在一个数组中作为`Promise.all`的`resolve`的结果
	- 数组中只要有一个`Promise`实例失败，`Promise.all`就失败。`Promise.all`的`.catch()`会捕获到这个`reject`
- 实现
	- ```js
	  Promise.MyAll = function (promises) {
	    let arr = [],
	      count = 0
	    return new Promise((resolve, reject) => {
	      promises.forEach((item, i) => {
	        Promise.resolve(item).then(res => {
	          arr[i] = res
	          count += 1
	          if (count === promises.length) resolve(arr)
	        }, reject)
	      })
	    })
	  }
	  ```