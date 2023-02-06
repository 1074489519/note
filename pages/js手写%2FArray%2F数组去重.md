title:: js手写/Array/数组去重

- ## 双层循环
	- ```js
	  function uniqeue(array) {
	    var res = []
	  
	    for (var i = 0, arrLen = array.length; i < arrLen; i++) {
	      for (var j = 0, resLen = res.length; j < resLen; j++) {
	        if (array[i] === res[j]) break
	      }
	      // 如果array[i]是唯一的，那么执行完循环，j等于resLen
	      if (j === resLen) res.push(array[i])
	    }
	    return res
	  }
	  
	  var arr = [1, 1, "1", "1"]
	  console.log(uniqeue(arr))
	  // [1, "1"]
	  ```
- ## indexOf
	- ```js
	  function uniqeue(array) {
	    var res = []
	  
	    for (var i = 0, arrLen = array.length; i < arrLen; i++) {
	      if (res.indexOf(array[i]) === -1) {
	        res.push(array[i])
	      }
	    }
	    return res
	  }
	  
	  var arr = [1, 1, "1", "1", 2]
	  console.log(uniqeue(arr)) // [ 1, '1', 2 ]
	  ```
- ## 排序后去重
	- ```js
	  function uniqeue(array) {
	    var sortArray = array.concat().sort()
	    var res = [sortArray[0]]
	    for (var i = 1; i < sortArray.length; i++) {
	      if (sortArray[i] !== sortArray[i - 1]) {
	        res.push(sortArray[i])
	      }
	    }
	    return res
	  }
	  
	  var arr = [3, 1, 1, "1", "1", 2]
	  console.log(uniqeue(arr)) // [ 1, '1', 2, 3 ]
	  ```
- ## filter
	- ```js
	  function uniqeue(array) {
	    return array.filter(function (item, index, array) {
	      return array.indexOf(item) === index
	    })
	  }
	  
	  var arr = [3, 1, 1, "1", "1", 2]
	  console.log(uniqeue(arr))
	  // [ 3, 1, '1', 2 ]
	  ```
	- 排序去重：
		- ```js
		  function uniqeue(array) {
		    return array
		      .concat()
		      .sort()
		      .filter(function (item, index, array) {
		        return !index || item !== array[index - 1]
		      })
		  }
		  
		  var arr = [3, 1, 1, "1", "1", 2, 3]
		  console.log(uniqeue(arr))
		  // [ 1, '1', 2, 3 ]
		  ```
- ## Object键值对
	- ```js
	  function uniqeue(array) {
	    var obj = {}
	  
	    return array.filter(function (item, index, array) {
	      console.log(typeof item + JSON.stringify(item)) // 避免 1 和 "1" 冲突的问题
	      return obj.hasOwnProperty(typeof item + JSON.stringify(item))
	        ? false
	        : (obj[typeof item + JSON.stringify(item)] = true)
	    })
	  }
	  
	  var arr = [3, 1, 1, "1", "1", 2, 3]
	  console.log(uniqeue(arr))
	  // [ 3, 1, '1', 2 ]
	  ```
- ## ES6
	- ```js
	  function uniqeue(array) {
	    return Array.from(new Set(array))
	    // or
	    // return [...new Set(array)]
	  }
	  
	  var arr = [3, 1, 1, "1", "1", 2, 3]
	  console.log(uniqeue(arr))
	  // [ 3, 1, '1', 2 ]
	  ```