- 题目
	- [8. 字符串转换整数 (atoi)](https://leetcode.cn/problems/string-to-integer-atoi/description/)
- 题解
	- ```js
	  /**
	   * @param {string} s
	   * @return {number}
	   */
	  var myAtoi = function (s) {
	      let length = s.length, p = 0, res = 0, flag = 1
	  
	      while (p < length && s[p] === ' ') p++
	  
	      if (s[p] === '-') {
	          flag = -1
	          p++
	      } else if (s[p] === '+') {
	          p++
	      }
	  
	      for (; p < length; p++) {
	          const num = s[p] - '0'
	          if (s[p] !== ' ' && num >= 0) {
	              const max = Math.pow(2, 31) - 1
	              const min = Math.pow(-2, 31)
	              if (res > parseInt(max / 10) || res === parseInt(max / 10) && num >= max % 10) {
	                  return max
	              } else if (res < parseInt(min / 10) || res === parseInt(min / 10) && -num <= min % 10) {
	                  return min
	              } else {
	                  res = res * 10 + flag * num
	              }
	          } else {
	              break
	          }
	      }
	      return res
	  };
	  ```