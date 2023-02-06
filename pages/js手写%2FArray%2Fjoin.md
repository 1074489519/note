title:: js手写/Array/join

- 用处
	- 将数组用分隔符拼成字符串，分隔符默认为 `,`
- 实现
	- ```js
	  Array.prototype.myJoin = function (s = ",") {
	    let str = ""
	    for (let i = 0; i < this.length; i++) {
	      str = i === 0 ? `${str}${this[i]}` : `${str}${s}${this[i]}`
	    }
	    return str
	  }
	  ```