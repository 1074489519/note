title:: js手写/Array/splice

- 难点
	- 截取长度和替换长度的比较，不同情况
- 实现
	- ```js
	  Array.prototype.mySplice = function (start, length, ...values) {
	    if (length === 0) return 0
	    length = start + length > this.length - 1 ? this.length - start : length
	    console.log(length)
	    const res = [],
	      tmpArr = [...this]
	    for (let i = start; i < start + values.length; i++) {
	      this[i] = values[i - start]
	    }
	    this.length = start + values.length
	  
	    if (values.length < length) {
	      const cha = length - values.length
	      console.log(cha)
	      for (let i = start + values.length; i < tmpArr.length; i++) {
	        this[i] = tmpArr[i + cha]
	      }
	      this.length = this.length - cha
	    }
	    if (values.length > length) {
	      for (let i = start + length; i < tmpArr.length; i++) {
	        this.push(tmpArr[i])
	      }
	    }
	    for (let i = start; i < start + length; i++) {
	      res.push(tmpArr[i])
	    }
	    return res
	  }
	  ```
-