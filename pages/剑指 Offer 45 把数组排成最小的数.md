- # 题目描述
	- 输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。
	- **示例 1:**
	- ```
	  输入: [10,2]
	  输出: "102"
	  ```
	- **示例 2:**
	- ```
	  输入: [3,30,34,5,9]
	  输出: "3033459"
	  ```
- # 实现
	- ```js
	  /**
	   * @param {number[]} nums
	   * @return {string}
	   */
	  var minNumber = function(nums) {
	      quickSort(nums, 0, nums.length-1)
	      var sum = nums.reduce((prev, cur) => {
	          return prev + cur 
	      }, '')
	      return sum
	  };
	  var quickSort = function(nums, l, r) {
	      if(l > r) return 
	      var i = l, j = r 
	      while(i < j) {
	          while(i < j && ('' + nums[j] + nums[l]) >= ('' + nums[l] + nums[j])) j--
	          while(i < j && ('' + nums[i] + nums[l]) <= ('' + nums[l] + nums[i])) i++
	          swap(nums, i, j)
	      }
	      swap(nums, l, i)
	      quickSort(nums, l, j - 1)
	      quickSort(nums, j + 1, r)
	  }
	  var swap = function(nums, l, r) {
	      var tmp = nums[l]
	      nums[l] = nums[r]
	      nums[r] = tmp 
	  }
	  ```