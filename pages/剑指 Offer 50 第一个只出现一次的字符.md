- # 题目描述
	- 在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。
	- 示例 1:
	- ```
	  输入：s = "abaccdeff"
	  输出：'b'
	  ```
	- 示例 2:
	- ```
	  输入：s = "" 
	  输出：' '
	  ```
- # 实现
	- ```js
	  /**
	   * @param {string} s
	   * @return {character}
	   */
	  var firstUniqChar = function(s) {
	      var map = new Map() 
	      for(var c of s) {
	          map.set(c, !map.has(c))
	      }
	      for(var c of s) {
	          if(map.get(c)) return c 
	      }
	      return ' '
	  };
	  ```