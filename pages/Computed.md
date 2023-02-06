- ## 初始化过程
	- [[Vue]] 的计算属性在两个地方都有初始化过程，一个是在 `initState` 中，一个是在 `Vue.extend` 中
	- #### 在`initState`中
	- ```js
	  export function initState (vm: Component) {
	    // ...
	    
	    const opts = vm.$options
	    // ...
	    
	    if (opts.computed) initComputed(vm, opts.computed)
	    // ...
	  }
	  ```
	- 如果 `vm.$options` 中有 `computed` ，就会调用 `initComputed`
	- ```js
	  // computed Watcher 的 lazy 为 true
	  const computedWatcherOptions = { lazy: true }
	  
	  function initComputed (vm: Component, computed: Object) {
	    // 创建 vm._computedWatchers 对象
	    const watchers = vm._computedWatchers = Object.create(null)
	    const isSSR = isServerRendering()
	    // 遍历 computed 中的 属性
	    for (const key in computed) {
	      // 获取属性值
	      const userDef = computed[key]
	      const getter = typeof userDef === 'function' ? userDef : userDef.get
	      if (process.env.NODE_ENV !== 'production' && getter == null) {
	        warn(
	          `Getter is missing for computed property "${key}".`,
	          vm
	        )
	      }
	  
	      if (!isSSR) {
	        // 为 computed 属性创建 Watcher
	        // 创建 computedWatcher
	        watchers[key] = new Watcher(
	          vm,
	          getter || noop,
	          noop,
	          computedWatcherOptions
	        )
	      }
	  
	      // 通过 extend 方法创建组件函数（Sub）的时候，已经将 computed 属性挂载到了 Sub 的 prototype 上
	      // 在这里仅仅是定义实例化时定义的计算属性。比如 根组件的计算属性
	      // 并保证 computed 的属性名和 data、props 的属性名没有重复
	      if (!(key in vm)) {
	        defineComputed(vm, key, userDef)
	      } else if (process.env.NODE_ENV !== 'production') {
	        if (key in vm.$data) {
	          warn(`The computed property "${key}" is already defined in data.`, vm)
	        } else if (vm.$options.props && key in vm.$options.props) {
	          warn(`The computed property "${key}" is already defined as a prop.`, vm)
	        }
	      }
	    }
	  }
	  ```
	- `initComputed` 会为每个计算属性创建一个 ** `Computed Watcher` ** ，这里要注意的点是 `Computed Watcher` 的 `options.lazy` 为 `true` ，并将计算属性赋值给 `Watcher` 实例的 `getter` 属性。而创建的 `Computed Watcher` 会添加到 `vm._computedWatchers` 里面；然后执行 `defineComputed` 添加响应
	- 看下 `Computed Watcher` 和 `Render Watcher` 的区别
	- ```js
	  // Watcher类内部代码
	  
	  this.lazy = !!options.lazy
	  // ...
	  
	  this.dirty = this.lazy
	  // ...
	  
	  this.value = this.lazy ? undefined : this.get()
	  ```
	- 相对于 `Render Watcher` ， `Computed Watcher` 的 `lazy` 为 `true` ，并且 `dirty` 也为 `true` ；因为 `lazy` 为 `true` ，所以在创建 `Computed Watcher` 过程中并不会执行 `this.get()` 方法；也就不会获取计算属性的返回值。
		- 而 在创建 `Render Watcher` 过程中会执行 `this.get()` ，从而执行组件的 `render` 函数。也就是说计算属性的返回值不是在创建 `Computed Watcher` 时获取的。
	- #### 在  `Vue.extend`  中
	- ```js
	  function initComputed (Comp) {
	    const computed = Comp.options.computed
	    for (const key in computed) {
	      // 将组件的计算属性挂载到 组件构造函数函数的原型上
	      // 作用：当实例化时，就可以通过 this.key 的方式访问了
	      defineComputed(Comp.prototype, key, computed[key])
	    }
	  }
	  ```
	- `initComputed` 函数会对所有计算属性执行 `defineComputed` 方法。
	- `defineComputed` 定义在 `src/core/instance/state.js` 中
	- ```js
	  const sharedPropertyDefinition = {
	    enumerable: true,
	    configurable: true,
	    get: noop,
	    set: noop
	  }
	  
	  export function defineComputed (
	    target: any,
	    key: string,
	    userDef: Object | Function
	  ) {
	    // 如果不是 ssr 则为 true
	    const shouldCache = !isServerRendering()
	    // 设置 取描述符
	    // userDef 可能是一个函数，也可能是一个有 getter 和 setter 属性的对象
	    if (typeof userDef === 'function') {
	      // 设置取描述符
	      sharedPropertyDefinition.get = shouldCache
	        ? createComputedGetter(key)
	        : createGetterInvoker(userDef)
	      sharedPropertyDefinition.set = noop
	    } else {
	      // 设置取描述符
	      sharedPropertyDefinition.get = userDef.get
	        ? shouldCache && userDef.cache !== false
	          ? createComputedGetter(key)
	          : createGetterInvoker(userDef.get)
	        : noop
	      // 设置存描述符
	      sharedPropertyDefinition.set = userDef.set || noop
	    }
	    // 如果 计算属性没有设置 setter 方法，则对计算属性赋值时，报错（计算属性 key 没有分配 setter 方法）
	    if (process.env.NODE_ENV !== 'production' &&
	        sharedPropertyDefinition.set === noop) {
	      sharedPropertyDefinition.set = function () {
	        warn(
	          `Computed property "${key}" was assigned to but it has no setter.`,
	          this
	        )
	      }
	    }
	    // 添加拦截，让开发者可以通过 this.key(vm.key) 的方式访问
	    Object.defineProperty(target, key, sharedPropertyDefinition)
	  }
	  ```
	- `defineComputed` 方法通过 `Object.defineProperty` 将所有计算属性代理到 `vm` / `Sub.prototype` 上，并将 `createComputedGetter` 函数的返回值设置成**取描述符**； 将计算属性的 `set` 方法设置成**存描述符**
	- 接着来看 `createComputedGetter`
	- ```js
	  function createComputedGetter (key) {
	    return function computedGetter () {}
	  }
	  ```
	- `createComputedGetter` 函数的内部逻辑一会再看，现在就知道它返回一个函数就行，并且这个函数的执行时机是**获取该计算属性时触发**
	- #### 小结
	- ##### 组件  `computed`  的初始化
		- 对于组件 `computed` 的初始化，就是在创建组件构造函数时，通过 `Object.defineProperty` 方法将组件中所有计算属性添加到组件构造函数的原型对象上，并设置存取描述符。
		  
		  当创建组件实例时，为每个计算属性创建一个 `Computed Watcher` ，并将计算属性复制给 `Watcher` 实例的 `getter` 属性；并且开发环境下会判断 `computed` 中的 `key` 和 `data` 、 `props` 中的 `key` 是否重复。
	- ##### 根实例  `computed`  的初始化
		- 对于根实例 `computed` 的初始化，就比较简单了，就是获取计算属性，并给 `computed` 的每个 `key` 创建一个 `Computed Watcher` ，通过 `Object.defineProperty` 方法将所有计算属性挂载到组件实例上，并设置存取描述符。
- ## 响应式原理
	- #### 依赖收集
		- 组件执行 `render` 函数时，如果使用了某个计算属性，会触发该计算属性的 `getter` ，这个方法就是上面 `createComputedGetter` 中的返回值
		- ```js
		  function createComputedGetter (key) {
		    return function computedGetter () {
		      const watcher = this._computedWatchers && this._computedWatchers[key]
		      if (watcher) {
		        // 只做一次依赖收集
		        if (watcher.dirty) {
		          // 执行 定义的计算属性函数
		          watcher.evaluate()
		        }
		        if (Dep.target) {
		          // 将render watcher 添加到 依赖属性的 dep 中，当依赖属性修改后，通过 render watcher 的get方法去触发组件更新
		          watcher.depend()
		        }
		        return watcher.value
		      }
		    }
		  }
		  ```
		- 首先会根据 `key` 获取对应计算属性的 `Computed Watcher` ，因为在初始化过程中， `watcher.dirty` 是 `true` ，所以会执行 `[:span {} " "]` 方法
		- ```js
		  evaluate () {
		      this.value = this.get()
		      this.dirty = false
		  }
		  ```
		- `evaluate` 方法会执行 `this.get()` 方法，获取计算属性的返回值，并将当前 `Watcher` 的 `dirty` 置为 `false` ，从而防止多次执行 `this.get()` 方法。
		- `get` 方法在 `响应式原理` 一节中看过
		- ```js
		    get () {
		      pushTarget(this)
		      let value
		      const vm = this.vm
		      try {
		        value = this.getter.call(vm, vm)
		      } catch (e) {
		      } finally {
		        // ...
		        popTarget()
		        this.cleanupDeps()
		      }
		      return value
		    }
		  ```
		- 首先将 `Computed Watcher` 入栈，执行 `this.getter` 也就是计算属性的属性值并获取结果 `value` ，然后出栈，将依赖属性的 `dep` 添加到 `depIds` 和 `deps` 中，并将结果返回。
		- 在执行 `this.getter` 过程中，会获取计算属性中依赖属性的变量值，从而触发响应式变量的 `getter` ，将 `Computed Watcher` 添加到响应式变量的 `dep.subs` 中。
		- 回到 `createComputedGetter` ，此时 `Dep.target` 指向的是组件的 `Render Watcher` ，因为在执行组件 `render` 函数时，会将组件的 `Render Watcher` 入栈，当获取计算属性的属性值时会将 `Computed Watcher` 入栈，执行结束后 `Computed Watcher` 出栈，所以此时的 `Dep.target` 指向的是组件的 `Render Watcher` ；接下来执行 `Computed Watcher` 的 `depend` 方法
		- ```js
		  depend () {
		      let i = this.deps.length
		      while (i--) {
		        this.deps[i].depend()
		      }
		  }
		  ```
		- `depend` 方法内，遍历 `deps` ， `deps` 是一个存放当前计算属性 `Dep` 实例的数组，执行每个 `Dep` 实例的 `depend` 方法，将组件的 `Render Watcher` 添加到该计算属性所有依赖属性的 `dep.subs` 里面
		- `watcher.depend` 执行完成之后，会返回计算属性的返回值，到此依赖收集结束
		- #### 依赖收集小结
			- 计算属性的依赖收集过程其实是对使用到的响应式属性进行依赖收集
			- 当组件的 `render` 函数中使用了某个计算属性时，会执行计算属性，在这期间会将 `Computed Watcher` 、 `Render Watcher` 添加到计算属性依赖属性的 `dep.subs` 中
	- #### 更新
		- 当计算属性依赖的响应式属性修改时，会触发依赖属性的 `setter` 方法
		- ```js
		  set: function reactiveSetter (newVal) {
		    // ...
		    
		    dep.notify()
		  }
		  ```
		- `setter` 方法中，会通知所有 `Watcher` 更新，其中就包括 `Computed Watcher` 和 `Render Watcher` ；调用 `Watcher` 的 `update` 方法
		- ```js
		    update () {
		      /* istanbul ignore else */
		      if (this.lazy) {
		        this.dirty = true
		      } else if (this.sync) {
		        this.run()
		      } else {
		        queueWatcher(this)
		      }
		    }
		  ```
		- 对于 `Computed Watcher` 就是将 `dirty` 设为 `true` 。而 `Render Watcher` 会执行 `Watcher` 实例的 `run` 方法，从而重新执行组件的 `render` 函数，更新计算属性的返回值
		- `dirty` 的作用其实就是只在相关响应式属性发生改变时才会重新求值。如果重复获取计算属性的返回值，只要响应式属性没有发生变化，就不会重新求值。
		- 也就是说当响应式属性改变时，触发响应式属性的 `setter` ，通知 `Computed Watcher` 将 `dirty` 置为 `false` ；等再次获取时，会获取到最新值，并重新给响应式属性的 `dep.subs` 添加 `Watcher`