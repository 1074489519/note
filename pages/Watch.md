- ## 初始化过程
	- `watch` 的初始化过程也是发生在 `initState` 中
	- ```js
	  export function initState (vm: Component) {
	    vm._watchers = []
	    const opts = vm.$options
	    //...
	    
	    
	    if (opts.watch && opts.watch !== nativeWatch) {
	      initWatch(vm, opts.watch)
	    }
	  }
	  ```
	- 首先为 `vm` 添加一个 `_watchers` 数组，用来存放当前组件的 `watch` ；然后调用 `initWatch` 方法初始化 `watch` 所有属性
	- ```js
	  function initWatch (vm: Component, watch: Object) {
	    for (const key in watch) {
	      const handler = watch[key]
	      if (Array.isArray(handler)) {
	        for (let i = 0; i < handler.length; i++) {
	          createWatcher(vm, key, handler[i])
	        }
	      } else {
	        createWatcher(vm, key, handler)
	      }
	    }
	  }
	  ```
	- `initWatch` 对所有 `watch` 调用 `createWatcher`
	- ```js
	  function createWatcher (
	    vm: Component,
	    expOrFn: string | Function,
	    handler: any,
	    options?: Object
	  ) {
	    if (isPlainObject(handler)) {
	      // 处理参数
	      options = handler
	      handler = handler.handler
	    }
	    if (typeof handler === 'string') {
	      // handler 可以是方法名
	      handler = vm[handler]
	    }
	    return vm.$watch(expOrFn, handler, options)
	  }
	  ```
	- `createWatcher` 就是获取回调函数，并调用 `vm.$watch` 方法
	- ```js
	    Vue.prototype.$watch = function (
	      expOrFn: string | Function,
	      cb: any,
	      options?: Object
	    ): Function {
	      const vm: Component = this
	      // 如果通过 this.$watch 设置的监听，则会执行 createWatcher 获取回调函数
	      if (isPlainObject(cb)) {
	        return createWatcher(vm, expOrFn, cb, options)
	      }
	      options = options || {}
	      options.user = true
	      // 此时 user 为 true，说明创建的 Watcher 是一个 User Watcher
	      const watcher = new Watcher(vm, expOrFn, cb, options)
	      if (options.immediate) {
	        try {
	          // 如果 options.immediate 为 true，则立刻执行一次回调函数
	          cb.call(vm, watcher.value)
	        } catch (error) {
	          handleError(error, vm, `callback for immediate watcher "${watcher.expression}"`)
	        }
	      }
	      // 返回一个函数，作用是取消监听
	      return function unwatchFn () {
	        watcher.teardown()
	      }
	    }
	  ```
	- `Vue.prototype.$watch` 就是创建一个 `User Watcher` ，并判断 `options.immediate` 是否为 `true` ，如果为 `true` 则立即执行一次回调函数。最后会返回一个取消监听的函数。
	- 自此 `watch` 的初始化过程结束
	- #### 小结
		- `watch` 的初始化过程最终目的就是给每个 `watch` 创建一个 `User Watcher` ，在创建过程中会对被监听的属性做依赖收集
- ## 依赖收集
	- 在初始化过程中会为每个 `watch` 创建一个 `User Watcher` ，而创建过程中会对被监听属性做依赖收集
	- `const watcher = new Watcher(vm, expOrFn, cb, options)`
	- 先看下参数
	- ```js
	  vm 组件实例
	  expOrFn 被监听的属性名（xxx、'xxx.yyy'）
	  cb 回调函数
	  options { user: true, deep: [自定义配置项], async: [自定义配置项] }
	  ```
	- `User Watcher` 的创建过程
	- ```js
	    constructor (
	      vm: Component,
	      expOrFn: string | Function,
	      cb: Function,
	      options?: ?Object,
	      isRenderWatcher?: boolean
	    ) {
	      this.vm = vm
	      if (isRenderWatcher) {
	        // 如果不是渲染 Watcher 则不会将 _watcher 挂载到 vm 上
	        vm._watcher = this
	      }
	      vm._watchers.push(this)
	      // options
	      if (options) {
	        /**
	         * computedWatcher 的 lazy 为 true
	         * userWarcher 的 user 为 true
	         * deep、sync 是 watch 的配置项
	         */
	        this.deep = !!options.deep
	        this.user = !!options.user
	        this.lazy = !!options.lazy
	        this.sync = !!options.sync
	        this.before = options.before
	      } else {
	        this.deep = this.user = this.lazy = this.sync = false
	      }
	      this.cb = cb
	      this.id = ++uid // uid for batching
	      this.active = true
	      this.dirty = this.lazy // for lazy watchers
	      this.deps = []
	      this.newDeps = []
	      this.depIds = new Set()
	      this.newDepIds = new Set()
	      this.expression = process.env.NODE_ENV !== 'production'
	        ? expOrFn.toString()
	        : ''
	      if (typeof expOrFn === 'function') {
	        this.getter = expOrFn
	      } else {
	        // User Watcher 的 expOrFn 是一个字符串，代表被监听属性的属性名
	        this.getter = parsePath(expOrFn)
	        if (!this.getter) {
	          this.getter = noop
	          process.env.NODE_ENV !== 'production' && warn(
	            `Failed watching path: "${expOrFn}" ` +
	            'Watcher only accepts simple dot-delimited paths. ' +
	            'For full control, use a function instead.',
	            vm
	          )
	        }
	      }
	      // computed Watcher 的 lazy 属性为 true，即不会立刻执行 get 方法
	      // render Watcher 的 lazy 属性为 false，会立刻执行 get 方法，返回值为 undefined
	      // user Watcher 的 lazy 属性为 false，会立刻执行 get 方法，返回值为 被监听属性的属性值
	      this.value = this.lazy
	        ? undefined
	        : this.get()
	    }
	  ```
	- `User Watcher` 除了 `user` 属性为 `true` 外，还有 `deep` 、 `async` 两个属性，这两个属性都是 `watch` 的配置项。
	- 实例化一个 `Watcher` 时会判断 `expOrFn` 参数的数据类型，对于 `User Watcher` 而言， `expOrFn` 就是被监听的属性名，是一个字符串，所以会执行 `parsePath` 方法。
	- ```js
	  export function parsePath (path: string): any {
	    if (bailRE.test(path)) {
	      return
	    }
	    const segments = path.split('.')
	    return function (obj) {
	      for (let i = 0; i < segments.length; i++) {
	        if (!obj) return
	        obj = obj[segments[i]]
	      }
	      return obj
	    }
	  }
	  ```
	- `parsePath` 方法根据 `.` 将字符串切割成字符串数组，并返回一个函数，这个函数会赋值给 `User Watcher` 的 `getter` 属性；函数内部会依次获取数组中所有元素对应的属性值并返回该属性值
	- 假设被监听的属性名是 `a.b.c` ，则此函数会依次获取 `this.a` 、 `this.a.b` 、 `this.a.b.c` 的属性值
	- 回到 `User Watcher` 的创建过程，此时 `this.getter` 已经赋好值，接下来会去执行 `this.get` 方法；也就是说只有 `Computed Watcher` 在创建过程中不会执行 `this.get` 方法
	- 再看一遍 `get` 方法
	- ```js
	    get () {
	      pushTarget(this)
	      let value
	      const vm = this.vm
	      try {
	        value = this.getter.call(vm, vm)
	      } catch (e) {} finally {
	        if (this.deep) {
	          // 如果 deep 为 true，并且被监听属性是一个对象，则对象内的所有属性都做一次依赖收集
	          traverse(value)
	        }
	        popTarget()
	        this.cleanupDeps()
	      }
	      return value
	    }
	  ```
	- 其实不管是计算属性还是 `data` 、 `props` 、 `watch` 他们的 `get` 方法整体逻辑都是一样的，
		- 1. 当前 `Watcher` 入栈
		- 2. 执行 `this.getter` （**每类 `Watcher` 的 `getter` 属性不同**）
		- 3. 执行 `traverse` 方法 （**只有 `deep` 为 `true` 的 `User Watcher` 才会执行**）
		- 4. 当前 `Watcher` 出栈
		- 5. 处理 `Watcher` 的 `deps` 属性
		- 6. 返回 `value`
	- 对于 `User Watcher` ，他的 `getter` 是 `parsePath` 函数的返回值，在执行 `getter` 过程中，会获取被监听属性的属性值，从而触发被监听属性的 `getter` 方法，将 `User Watcher` 添加到此属性的 `dep.subs` 中。
	- 上述执行完成后，会判断 `deep` 是否为 `true` ，如果为 `true` ，执行 `traverse` 方法
	- ```js
	  const seenObjects = new Set()
	  
	  export function traverse (val: any) {
	    _traverse(val, seenObjects)
	    seenObjects.clear()
	  }
	  
	  function _traverse (val: any, seen: SimpleSet) {
	    let i, keys
	    const isA = Array.isArray(val)
	    if ((!isA && !isObject(val)) || Object.isFrozen(val) || val instanceof VNode) {
	      return
	    }
	    if (val.__ob__) {
	      const depId = val.__ob__.dep.id
	      if (seen.has(depId)) {
	        return
	      }
	      seen.add(depId)
	    }
	    if (isA) {
	      i = val.length
	      while (i--) _traverse(val[i], seen)
	    } else {
	      keys = Object.keys(val)
	      i = keys.length
	      while (i--) _traverse(val[keys[i]], seen)
	    }
	  }
	  ```
	- `traverse` 方法整体思路其实很简单，如果被监听的属性是一个对象，则把对象的所有属性都访问一遍，从而触发所有属性的依赖收集；将 `User Watcher` 添加到每个属性的 `dep.subs` 中，这样当某个属性修改时，会触发属性的 `setter` ，从而触发 `watch` 回调
- ## 触发回调
	- 当修改被监听属性的属性值时，触发属性的 `setter` ，通知 `dep.subs` 中所有 `Watcher` 更新，执行 `watcher.update` 方法
	- ```js
	    update () {
	      if (this.lazy) {
	        this.dirty = true
	      } else if (this.sync) {
	        this.run()
	      } else {
	        queueWatcher(this)
	      }
	    }
	  ```
	- 如果 `User Watcher` 的 `sync` 属性为 `true` ，立刻执行 `run` 方法；如果 `sync` 属性为 `false` ，通过 `queueWatcher(this)` 在下一次任务队列中执行 `User Watcher` 的 `run` 方法
	- 先看下 `run` 方法
	- ```js
	    run () {
	      if (this.active) {
	        const value = this.get()
	        if (
	          value !== this.value ||
	          isObject(value) ||
	          this.deep
	        ) {
	          // 当添加自定义 watcher 的时候能在回调函数的参数中拿到新旧值的原因
	          const oldValue = this.value
	          this.value = value
	          if (this.user) {
	            try {
	              this.cb.call(this.vm, value, oldValue)
	            } catch (e) {
	              handleError(e, this.vm, `callback for watcher "${this.expression}"`)
	            }
	          } else {
	            this.cb.call(this.vm, value, oldValue)
	          }
	        }
	      }
	    }
	  ```
	- 对于 `User Watcher` 的 `run` 方法，首先会调用 `this.get()` 重新让被监听属性做依赖收集，并获取最新值；如果最新值和老值不想等，调用回调函数，并将新老值传入
	- 上面的判断逻辑中除了判断新老值是否想等还会判断 `isObject(value) || this.deep` ，这是因为如果被监听的属性是一个对象/数组的话，修改对象/数组的属性后，新老值是相同的，所以为了防止出现这种情况导致回调不执行，从而增加这段逻辑
	- 接下来看下 `User Watcher` 在 `queueWatcher` 中是怎么被调用的；正常情况下，和 `data` 相同就是将 `User Watcher` 添加到队列中，并保证同一队列中每个 `User Watcher` 都是唯一的
	- #### 不同情况
		- **在 `watch` 回调中修改另一个被监听属性的值**
		- 他的执行逻辑如下：
		- ```js
		  export const MAX_UPDATE_COUNT = 100
		  let circular: { [key: number]: number } = {}
		  
		  function flushSchedulerQueue () {
		    currentFlushTimestamp = getNow()
		    flushing = true
		    let watcher, id
		    queue.sort((a, b) => a.id - b.id)
		  
		    for (index = 0; index < queue.length; index++) {
		      watcher = queue[index]
		      // 执行组件的 beforeUpdate 钩子, 先父后子
		      if (watcher.before) {
		        watcher.before()
		      }
		      id = watcher.id
		      has[id] = null
		      watcher.run()
		      if (process.env.NODE_ENV !== 'production' && has[id] != null) {
		        circular[id] = (circular[id] || 0) + 1
		        if (circular[id] > MAX_UPDATE_COUNT) {
		          warn(
		            'You may have an infinite update loop ' + (
		              watcher.user
		                ? `in watcher with expression "${watcher.expression}"`
		                : `in a component render function.`
		            ),
		            watcher.vm
		          )
		          break
		        }
		      }
		    }
		  ```
		- 在下一个队列中执行 `flushSchedulerQueue` 方法
			- `flushing` 置为 `true` ，说明正在更新队列中的 `Watcher` ；
			- 队列排序，保证 `Watcher` 更新顺序；
			- 遍历队列，更新队列中的所有 `Watcher`
			- `has[id] = null` ，将正在更新的 `Watcher` 从 `has` 中去掉；
			- 执行 `User Watcher` 的 `run` 方法；
			- 执行第一个 `watch` 的回调，回调内修改被监听属性的值，触发属性的 `setter` ，将监听这个属性的 `User Wather` 通过 `queueWatcher` 添加到队列中，此时和之前就有差别了
		- ```js
		  export function queueWatcher (watcher: Watcher) {
		    const id = watcher.id
		    if (has[id] == null) {
		      has[id] = true
		      // 此时 flushing 是 true
		      if (!flushing) {
		        queue.push(watcher)
		      } else {
		        let i = queue.length - 1
		        while (i > index && queue[i].id > watcher.id) {
		          i--
		        }
		        queue.splice(i + 1, 0, watcher)
		      }
		      // queue the flush
		      if (!waiting) {
		        waiting = true
		  
		        if (process.env.NODE_ENV !== 'production' && !config.async) {
		          flushSchedulerQueue()
		          return
		        }
		        nextTick(flushSchedulerQueue)
		      }
		    }
		  }
		  ```
		- 因为在上面已经将 `flushing` 置为 `true` 了，所以会走 `else` 逻辑； `else` 逻辑就是遍历队列，并将 `Watcher` 添加到对应位置。位置逻辑如下
		- ```js
		  1. 组件更新是从父到子。(因为父组件总是在子组件之前创建)
		  2. User Watcher 在 Render Watcher 之前执行
		  3. 如果一个组件在父组件的 Watcher 执行期间被销毁，那么它对应 Watcher 执行都可以被跳过，所以父组件的 watcher 应该先执行
		  ```
		- 添加到对应位置后，因为 `waiting` 已经是 `true` 了，所以不会再次执行 `nextTick(flushSchedulerQueue)` ，而是回到 `flushSchedulerQueue` 方法继续循环
	- **在 `watch` 回调内修改当前被监听属性的值**
		- 其实整体逻辑和上面说的一样，但是会多一步，就是在 `flushSchedulerQueue` 方法的循环里面
		- 因为新添加的 `User Watcher` 和刚执行完的 `User Watcher` 是同一个 `Watcher` ，所以接下来触发的 `if` 条件在开发环境下是成立的
		- ```js
		  if (process.env.NODE_ENV !== 'production' && has[id] != null) {
		    circular[id] = (circular[id] || 0) + 1
		    if (circular[id] > MAX_UPDATE_COUNT) {
		      warn(
		        'You may have an infinite update loop ' + (
		          watcher.user
		            ? `in watcher with expression "${watcher.expression}"`
		            : `in a component render function.`
		        ),
		        watcher.vm
		      )
		      break
		    }
		  }
		  ```
		- 此时 `circular[id]` 数量会加一，并按照上面的逻辑一直重复执行，直到总数量大于 `MAX_UPDATE_COUNT` 时会报错