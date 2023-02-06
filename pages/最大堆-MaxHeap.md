- ## 实现代码
	- ```js
	  class MaxHeap {
	    #data
	    constructor() {
	      // 把传入数组转换成堆
	      if (Array.isArray(arguments[0])) {
	        this.#data = arguments[0]
	        for (let i = this.#parent(this.#data.length - 1); i >= 0; i--) {
	          this.#siftDown(i)
	        }
	      } else {
	        this.#data = []
	      }
	    }
	    size() {
	      return this.#data.length
	    }
	    isEmpty() {
	      return this.size() === 0
	    }
	    #parent(index) {
	      if (index === 0) throw new Error("index-0 doesn't has parent")
	      return parseInt((index - 1) / 2)
	    }
	    #leftChild(index) {
	      return parseInt(index * 2 + 1)
	    }
	    #rightChild(index) {
	      return parseInt(index * 2 + 2)
	    }
	    add(e) {
	      this.#data.push(e)
	      // 从最后插入元素，再跟父元素比较，如果比父大，则交换位置，直到小于或者已经到堆顶
	      this.#siftUp(this.size() - 1)
	    }
	    #siftUp(k) {
	      while (k > 0 && this.#data[k] > this.#data[this.#parent(k)]) {
	        this.#swap(this.#data, k, this.#parent(k))
	        k = this.#parent(k)
	      }
	    }
	    findMax() {
	      if (this.size() === 0) throw new Error("can not findMax")
	      return this.#data[0]
	    }
	    extractMax() {
	      // 取出最大元素
	      const ret = this.findMax()
	  
	      this.#swap(this.#data, 0, this.size() - 1)
	      this.#data.length = this.#data.length - 1
	      this.#siftDown(0)
	  
	      return ret
	    }
	    #siftDown(k) {
	      // 看当前节点是否有子节点
	      while (this.#leftChild(k) < this.size()) {
	        let j = this.#leftChild(k)
	  
	        // 如果有右节点，判断一下是不是大于做节点，如果大于赋值j
	        if (j + 1 < this.size() && this.#data[j + 1] > this.#data[j]) j++
	  
	        if (this.#data[k] >= this.#data[j]) break
	  
	        this.#swap(this.#data, k, j)
	        k = j
	      }
	    }
	    // 替换堆顶元素
	    replace(e) {
	      const ret = this.findMax()
	  
	      this.#data[0] = e
	      this.#siftDown(0)
	  
	      return ret
	    }
	  
	    #swap(arr, i, j) {
	      if (i >= arr.length || j >= arr.length) throw new Error("index is illegal")
	      const tmp = arr[i]
	      arr[i] = arr[j]
	      arr[j] = tmp
	    }
	  
	    print() {
	      console.log(this.#data)
	    }
	  }
	  
	  const arr = [3, 1, 2, 8, 5, 6, 0]
	  const heap = new MaxHeap(arr)
	  // heap.add(2)
	  // heap.add(1)
	  // heap.add(5)
	  // heap.add(6)
	  // heap.add(3)
	  
	  heap.print()
	  
	  // heap.extractMax()
	  // heap.extractMax()
	  
	  // heap.replace(4)
	  
	  // heap.print()
	  
	  ```
- ## 堆排序
	- ```js
	  class HeapSort {
	    static sort(data) {
	      const maxHeap = new MaxHeap()
	      for (let item of data) {
	        maxHeap.add(item)
	      }
	      for (let i = 0; i < data.length; i++) {
	        data[i] = maxHeap.extractMax()
	      }
	      console.log(data)
	    }
	  }
	  
	  // 测试代码
	  const arr = [3, 1, 2, 8, 5, 6, 0]
	  HeapSort.sort(arr)
	  ```
- ## Heapify
	- > 将任意数组整理成堆的形状
	- 找到最后一个非叶子节点的位置，向前做下沉操作
	- ```js
	  class MaxHeap {
	    constructor() {
	      // 把传入数组转换成堆
	      if (Array.isArray(arguments[0])) {
	        this.#data = arguments[0]
	        for (let i = this.#parent(this.#data.length - 1); i >= 0; i--) {
	          this.#siftDown(i)
	        }
	      } else {
	        this.#data = []
	      }
	    }
	    ...
	  }
	  ```