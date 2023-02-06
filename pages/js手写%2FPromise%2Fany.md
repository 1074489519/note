title:: js手写/Promise/any

- 概述
	- `Promise.any`与`Promise.all`相反。`Promise.any`中只要有一个`Promise`实例成功就成功，只有当所有`Promise`实例失败时，`Promise.any`才失败，此时`Promise.any`会把所有的失败/错误集合在一起，返回一个失败的`Promise`和[ `AggregateError` ](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FAggregateError)类型的实例。MDN 上说这个方法还处于试验阶段，如果 `node` 或者浏览器版本过低可能无法使用，各位看官自行测试下。
- 实现
	- ```js
	  Promise.MyAny = function (promises) {
	    let arr = [],
	      count = 0
	    return new Promise((resolve, reject) => {
	      promises.forEach((item, i) => {
	        Promise.resolve(item).then(resolve, err => {
	          arr[i] = { status: 'rejected', val: err }
	          count += 1
	          if (count === promises.length) reject(new Error('没有promise成功'))
	        })
	      })
	    })
	  }
	  ```