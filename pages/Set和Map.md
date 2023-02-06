- ## Set实现
	- ```js
	  const BST = require('./tree')
	  
	  class BSTSet {
	      #bst
	      constructor() {
	          this.#bst = new BST()
	      }
	  
	      size() {
	          return this.#bst.size() 
	      }
	  
	      isEmpty() {
	          return this.#bst.isEmpty() 
	      }
	  
	      add(e) {
	          this.#bst.add(e)
	      }
	  
	      contains() {
	          return this.#bst.contains()
	      }
	  
	      print() {
	          this.#bst.print()
	      }
	  }
	  
	  
	  const set = new BSTSet()
	  set.add(1)
	  set.add(2)
	  
	  console.log(set.size())
	  set.print()
	  ```
- ## Map实现
	- ```js
	  class BSTMap {
	    #Node = class {
	      constructor(key, value) {
	        this.key = key
	        this.value = value
	        this.left = null
	        this.right = null
	      }
	    }
	  
	    #root
	    #size
	    constructor() {
	      this.#size = 0
	    }
	  
	    size() {
	      return this.#size
	    }
	    isEmpty() {
	      return this.#size === 0
	    }
	  
	    add(key, value) {
	      root = this.#add(this.#root, key, value)
	    }
	    #add(node, key, value) {
	      if (node == null) {
	        size++
	        return new this.#Node(key, value)
	      }
	  
	      if (key < node.key) {
	        node.left = this.#add(node.left, key, value)
	      } else if (key > node.key) {
	        node.right = this.#add(node.right, key, value)
	      } else {
	        node.value = value
	      }
	  
	      return node
	    }
	  
	    contains(key) {
	      return this.#getNode(this.#root, key) !== null
	    }
	    get(key) {
	      const node = this.#getNode(this.#root, key)
	      return node != null ? node.value : null
	    }
	    #getNode(node, key) {
	      if (node === null) return null
	  
	      if (key === node.key) {
	        return node
	      } else if (key < node.key) {
	        return this.#getNode(node.left, key)
	      } else {
	        return this.#getNode(node.right, key)
	      }
	    }
	  
	    set(key, value) {
	      const node = this.#getNode(this.#root, key)
	      if (node !== null) {
	        node.value = value
	      } else {
	        throw new Error("key is null")
	      }
	    }
	    remove(key) {
	      const node = this.#getNode(this.#root, key)
	      if (node != null) {
	        root = this.#remove(this.#root, key)
	        return node.value
	      }
	    }
	    #remove(node, key) {
	      if (node === null) {
	        return null
	      }
	      if (key < node.key) {
	        node.left = this.#remove(node.left, key)
	        return node
	      } else if (key > node.key) {
	        node.right = this.#remove(node.right, key)
	        return node
	      } else {
	        if (node.left === null) {
	          const rightNode = node.right
	          node.right = null
	          this.#size--
	          return rightNode
	        } else if (node.right === null) {
	          const leftNode = node.left
	          node.left = null
	          this.#size--
	          return leftNode
	        } else {
	          const successor = this.minimum(node.right)
	          successor.right = this.removeMin(node.right)
	          successor.left = node.left
	          node.left = node.right = null
	          return successor
	        }
	      }
	    }
	    minimum(node) {
	      if (node.left === null) {
	        return node
	      } else {
	        return this.minimum(node.left)
	      }
	    }
	  
	    removeMin(node) {
	      if (node.left === null) {
	        var rightNode = node.right
	        node.right = null
	        this.#size--
	        return rightNode
	      }
	      node.left = this.#removeMin(node.left)
	      return node
	    }
	  }
	  
	  ```