title:: js手写/Array/flat

- ## 特性
	- > 注：数组拍平方法 `Array.prototype.flat()` 也叫数组扁平化、数组拉平、数组降维。 本文统一叫：数组拍平
	- ```js
	  const animals = ["🐷", ["🐶", "🐂"], ["🐎", ["🐑", ["🐲"]], "🐛"]];
	  
	  // 不传参数时，默认“拉平”一层
	  animals.flat();
	  // ["🐷", "🐶", "🐂", "🐎", ["🐑", ["🐲"]], "🐛"]
	  
	  // 传入一个整数参数，整数即“拉平”的层数
	  animals.flat(2);
	  // ["🐷", "🐶", "🐂", "🐎", "🐑", ["🐲"], "🐛"]
	  
	  // Infinity 关键字作为参数时，无论多少层嵌套，都会转为一维数组
	  animals.flat(Infinity);
	  // ["🐷", "🐶", "🐂", "🐎", "🐑", "🐲", "🐛"]
	  
	  // 传入 <=0 的整数将返回原数组，不“拉平”
	  animals.flat(0);
	  animals.flat(-10);
	  // ["🐷", ["🐶", "🐂"], ["🐎", ["🐑", ["🐲"]], "🐛"]];
	  
	  // 如果原数组有空位，flat()方法会跳过空位。
	  ["🐷", "🐶", "🐂", "🐎",,].flat();
	  // ["🐷", "🐶", "🐂", "🐎"]
	  
	  ```
	- `Array.prototype.flat()` 用于将嵌套的数组“拉平”，编程一维数组。该方法返回一个新数组，对原数据没有影响。
	- 不传参数时，默认“拉平”一层，可以传入一个整数，表示想要“拉平”的层数。
	- 传入`<=0`的数组将返回原数组，不“拉平”。
	- `Infinity` 关键字作为参数时，无论多少层嵌套，都会转为一维数组。
	- 如果原数组有空位，`Array.ptototype.flat()` 会跳过空位。
- ## 实现一个简单的数组拍平`flat`函数
	- 思路：
		- 在数组中找到时数组类型的元素，然后将他们展开
	- 方法：
		- 遍历数组中的每一个元素
		- 判断元素是否是元素
		- 将数组的元素展开一层
	- ```js
	  function flat(arr) {
	    let arrResult = []
	  
	    arr.forEach((item) => {
	      if (Array.isArray(item)) {
	        // 递归
	        //   arrResult = arrResult.concat(arguments.callee(item))
	        // 或者用扩展运算符
	        arrResult.push(...arguments.callee(item))
	      } else {
	        arrResult.push(item)
	      }
	    })
	  
	    return arrResult
	  }
	  
	  const array = [1, 2, 3, [4, 5, 6, [7, 8, 9, { a: 1 }]]]
	  console.log(flat(array))
	  /*
	  [ 1, 2, 3, 4, 5, 6, 7, 8, 9, { a: 1 } ]
	  */
	  ```
- ## 用`reduce`实现`flat`函数
	- ```js
	  const flat = (arr) =>
	    arr.reduce((pre, cur) => pre.concat(Array.isArray(cur) ? flat(cur) : cur), [])
	  
	  const array = [1, 2, 3, [4, 5, 6, [7, 8, 9, { a: 1 }]]]
	  console.log(flat(array))
	  /*
	  [ 1, 2, 3, 4, 5, 6, 7, 8, 9, { a: 1 } ]
	  */
	  ```
- ## 使用栈的思想实现`flat`函数
	- ```js
	  function flat(arr) {
	    const stack = [arr]
	    let result = []
	    while (stack.length > 0) {
	      const cur = stack.pop()
	      if (Array.isArray(cur)) {
	        stack.push(...cur) //如果是数组再次入栈，并且展开了一层
	      } else {
	        result.unshift(cur) //如果不是数组就将其取出来放入结果数组中
	      }
	    }
	    return result
	  }
	  
	  const array = [1, 2, 3, [4, 5, 6, [7, 8, 9, { a: 1 }]]]
	  console.log(flat(array))
	  /*
	  [ 1, 2, 3, 4, 5, 6, 7, 8, 9, { a: 1 } ]
	  */
	  ```
- ## 通过传入整数参数控制“拉平”层数
	- ```js
	  const flat = (arr, num = 1) =>
	    num > 0
	      ? arr.reduce(
	          (pre, cur) => pre.concat(Array.isArray(cur) ? flat(cur, num - 1) : cur),
	          [],
	        )
	      : arr.slice()
	  
	  const array = [1, 2, 3, [4, 5, 6, [7, 8, 9, { a: 1 }]]]
	  console.log(flat(array))
	  /*
	  [ 1, 2, 3, 4, 5, 6, [ 7, 8, 9, { a: 1 } ] ]
	  */
	  ```
- ## 使用`generator`实现`flat`函数
	- ```js
	  function* flat(arr, num = 1) {
	    for (const item of arr) {
	      if (Array.isArray(item) && num > 0) {
	        yield* flat(item, num - 1)
	      } else {
	        yield item;
	      }
	    }
	  }
	  // 调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象。
	  // 也就是遍历器对象（Iterator Object）。所以我们要用一次扩展运算符得到结果
	  const array = [1, 2, 3, [4, 5, 6, [7, 8, 9, { a: 1 }]], [2, 3]]
	  console.log([...flat(array)])
	  /*
	  [ 1, 2, 3, 4, 5, 6, [ 7, 8, 9, { a: 1 } ], 2, 3 ]
	  *.
	  ```
- ## 实现在原型链上重写`flat`函数
	- ```js
	  Array.prototype.fakeFlat = function (num = 1) {
	    let arr = this.concat()
	  
	    while (num > 0) {
	      if (arr.some((item) => Array.isArray(item))) {
	        arr = [].concat.apply([], arr)
	      } else {
	        break
	      }
	      num--
	    }
	    return arr
	  }
	  
	  const array = [1, 2, 3, [4, 5, 6, [7, 8, 9, { a: 1 }]], [2, 3]]
	  console.log(array.fakeFlat(Infinity))
	  /*
	  [ 1, 2, 3, 4, 5, 6, 7, 8, 9, { a: 1 }, 2, 3 ]
	  */
	  ```
- ## 考虑数据空位的情况
	- ES5 大多数数组方法对空位的处理都会选择跳过空位包括： `forEach()` , `filter()` , `reduce()` , `every()` 和 `some()` 都会跳过空位。
	- ```js
	  Array.prototype.fakeFlat = function (num = 1) {
	    let arr = this.concat()
	  
	    return num > 0
	      ? arr.reduce(
	          (pre, cur) =>
	            pre.concat(Array.isArray(cur) ? cur.fakeFlat(--num) : cur),
	          [],
	        )
	      : arr.slice()
	  }
	  
	  const array = [1, 2, 3, , , [4, 5, 6, [7, 8, 9, { a: 1 }]], [2, 3]]
	  console.log(array.fakeFlat(Infinity))
	  /*
	  [ 1, 2, 3, 4, 5, 6, 7, 8, 9, { a: 1 }, 2, 3 ]
	  */
	  ```
	- #### 空位规则
		- **ES5 对空位的处理，就非常不一致，大多数情况下会忽略空位。**
			- `forEach()` , `filter()` , `reduce()` , `every()` 和 `some()` 都会跳过空位。
			- `map()` 会跳过空位，但会保留这个值。
			- `join()` 和 `toString()` 会将空位视为 `undefined` ，而 `undefined` 和 `null` 会被处理成空字符串。
		- **ES6 明确将空位转为 `undefined` 。**
			- `entries()` 、 `keys()` 、 `values()` 、 `find()` 和 `findIndex()` 会将空位处理成 `undefined` 。
			- `for...of` 循环会遍历空位。
			- `fill()` 会将空位视为正常的数组位置。
			- `copyWithin()` 会连空位一起拷贝。
			- 扩展运算符（ `...` ）也会将空位转为 `undefined` 。
			- `Array.from` 方法会将数组的空位，转为 `undefined` 。
-