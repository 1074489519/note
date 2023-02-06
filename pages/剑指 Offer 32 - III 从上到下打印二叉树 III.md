- # 题目描述
	- 请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。
	- 例如:
		- 给定二叉树: [3,9,20,null,null,15,7],
		- ```
		  3
		   / \
		  9  20
		    /  \
		   15   7
		  ```
		- 返回其层次遍历结果：
		- ```
		  [
		    [3],
		    [20,9],
		    [15,7]
		  ]
		  ```
- # 题目解析
	- #### 层序遍历 + 倒序
		- 此方法的优点是只用列表即可，无需其他数据结构。
		  偶数层倒序： 若 res 的长度为 奇数 ，说明当前是偶数层，则对 tmp 执行 倒序 操作。
- # 复杂度分析：
	- 时间复杂度 O(N) ： NN 为二叉树的节点数量，即 BFS 需循环 N 次，占用 O(N) 。共完成 少于 N 个节点的倒序操作，占用 O(N) 。
	- 空间复杂度 O(N) ： 最差情况下，即当树为满二叉树时，最多有 N/2 个树节点同时在 queue 中，使用 O(N) 大小的额外空间。
- # 实现
	- ```js
	  /**
	   * Definition for a binary tree node.
	   * function TreeNode(val) {
	   *     this.val = val;
	   *     this.left = this.right = null;
	   * }
	   */
	  /**
	   * @param {TreeNode} root
	   * @return {number[][]}
	   */
	  var levelOrder = function(root) {
	      var queue = [], res = []
	  
	      if(root) queue.push(root)
	      while(queue.length) {
	          var tmp = []
	          for(var i=queue.length; i>0; --i) {
	              var node = queue.shift()
	              tmp.push(node.val)
	              if(node.left) queue.push(node.left)
	              if(node.right) queue.push(node.right)
	          }
	          if(res.length % 2 === 1) tmp.reverse()
	          res.push(tmp)
	      }
	      return res
	  
	  };
	  ```