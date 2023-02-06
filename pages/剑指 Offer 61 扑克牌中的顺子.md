- # 题目描述
	- 从若干副扑克牌中随机抽 5 张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。
	- 示例 1:
	- ```
	  输入: [1,2,3,4,5]
	  输出: True
	  ```
	- 示例 2:
	- ```
	  输入: [0,0,1,2,5]
	  输出: True
	  ```
	- **限制：**
	- 数组长度为 5
	- 数组的数取值为 [0, 13] .
- # 实现
	- #### 方法一： 集合 Set + 遍历
	- ```js
	  /**
	   * @param {number[]} nums
	   * @return {boolean}
	   */
	  var isStraight = function(nums) {
	      var set = new Set()
	      var min = 14, max = 0
	      for(var n of nums) {
	          if(n === 0) continue
	          if(set.has(n)) return false 
	          min = Math.min(min, n)
	          max = Math.max(max, n)
	          set.add(n)
	      }
	      return max - min < 5
	  };
	  ```
	- #### 方法二：排序 + 遍历
	- ```js
	  /**
	   * @param {number[]} nums
	   * @return {boolean}
	   */
	  var isStraight = function(nums) {
	      nums.sort((a, b) => a - b)
	      var joker = 0
	      for(var i=0; i <= nums.length -1; i++) {
	          if(nums[i] === 0) joker++
	          else if(nums[i] === nums[i+1]) return false 
	      }
	      return nums[4] - nums[joker] < 5
	  };
	  ```