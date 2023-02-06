title:: js手写/Number/大数相加

- ```js
  const num1 = "12345678999999999"
  const num2 = "723592231231231"
  
  function add(a, b) {
    const maxLength = Math.max(a.length, b.length)
  
    a = a.padStart(maxLength, 0)
    b = b.padStart(maxLength, 0)
  
    let t = 0
    let f = 0 // 进位
    let sum = ""
    for (let i = maxLength - 1; i >= 0; i--) {
      t = parseInt(a[i]) + parseInt(b[i]) + f
      f = Math.floor(t / 10)
      sum = (t % 10) + sum
    }
    if (f === 1) {
      sum = "1" + sum
    }
    return sum
  }
  console.log("sum", add(num1, num2))
  
  ```
- 其它参考：
	- [bignumber.js](https://github.com/MikeMcl/bignumber.js/)