- > 懒加载或者按需加载，是一种很好的优化网页或应用的方式。这种方式实际上是先把你的代码在一些逻辑断点处分离开，然后在一些代码块中完成某些操作后，立即引用或即将引用另外一些新的代码块。这样加快了应用的初始加载速度，减轻了它的总体体积，因为某些代码块可能永远不会被加载。
	- #### 根据文件内容生成文件名，结合 import 动态引入组件实现按需加载
	  
	  通过配置 output 的 filename 属性可以实现这个需求。filename 属性的值选项中有一个 [contenthash]，它将根据文件内容创建出唯一 hash。当文件内容发生变化时，[contenthash] 也会发生变化。
	- ```
	  output: {
	  	filename: '[name].[contenthash].js',
	    chunkFilename: '[name].[contenthash].js',
	    path: path.resolve(__dirname, '../dist'),
	  },
	  ```
- #### 提取第三方库
	- 由于引入的第三方库一般都比较稳定，不会经常改变。所以将它们单独提取出来，作为长期缓存是一个更好的选择。
	  这里需要使用 webpack4 的 splitChunk 插件 cacheGroups 选项。
	  
	  ```
	  optimization: {
	  	runtimeChunk: {
	        name: 'manifest' // 将 webpack 的 runtime 代码拆分为一个单独的 chunk。
	    },
	    splitChunks: {
	        cacheGroups: {
	            vendor: {
	                name: 'chunk-vendors',
	                test: /[\\/]node_modules[\\/]/,
	                priority: -10,
	                chunks: 'initial'
	            },
	            common: {
	                name: 'chunk-common',
	                minChunks: 2,
	                priority: -20,
	                chunks: 'initial',
	                reuseExistingChunk: true
	            }
	        },
	    }
	  },
	  ```
	- test: 用于控制哪些模块被这个缓存组匹配到。原封不动传递出去的话，它默认会选择所有的模块。可以传递的值类型：RegExp、String和Function;
	- priority：表示抽取权重，数字越大表示优先级越高。因为一个 module 可能会满足多个 cacheGroups 的条件，那么抽取到哪个就由权重最高的说了算；
	- reuseExistingChunk：表示是否使用已有的 chunk，如果为 true 则表示如果当前的 chunk 包含的模块已经被抽取出去了，那么将不会重新生成新的。
	- minChunks（默认是1）：在分割之前，这个代码块最小应该被引用的次数（译注：保证代码块复用性，默认配置的策略是不需要多次引用也可以被分割）
	- chunks (默认是async) ：initial、async和all
	- name(打包的chunks的名字)：字符串或者函数(函数可以根据条件自定义名字)
- # 减少 ES6 转为 ES5 的冗余代码
	- Babel 转化后的代码想要实现和原来代码一样的功能需要借助一些帮助函数，比如：
	- ```
	  class Person {}
	  ```
	- 会被转换为：
	- ```
	  "use strict";
	  
	  function _classCallCheck(instance, Constructor) {
	    if (!(instance instanceof Constructor)) {
	      throw new TypeError("Cannot call a class as a function");
	    }
	  }
	  
	  var Person = function Person() {
	    _classCallCheck(this, Person);
	  };
	  ```
	- 这里  `_classCallCheck`  就是一个  `helper`  函数，如果在很多文件里都声明了类，那么就会产生很多个这样的  `helper`  函数。
	- 这里的 `@babel/runtime` 包就声明了所有需要用到的帮助函数，而 `@babel/plugin-transform-runtime` 的作用就是将所有需要 `helper` 函数的文件，从 `@babel/runtime包` 引进来：
	- ```
	  "use strict";
	  
	  var _classCallCheck2 = require("@babel/runtime/helpers/classCallCheck");
	  
	  var _classCallCheck3 = _interopRequireDefault(_classCallCheck2);
	  
	  function _interopRequireDefault(obj) {
	    return obj && obj.__esModule ? obj : { default: obj };
	  }
	  
	  var Person = function Person() {
	    (0, _classCallCheck3.default)(this, Person);
	  };
	  ```
	- 这里就没有再编译出 `helper` 函数 `classCallCheck` 了，而是直接引用了 `@babel/runtime` 中的 `helpers/classCallCheck` 。
	- **安装**
	- ```
	  npm i -D @babel/plugin-transform-runtime @babel/runtime
	  ```
	  **使用**
	  在  `.babelrc`  文件中
	  ```
	  "plugins": [
	          "@babel/plugin-transform-runtime"
	  ]
	  ```
	- 参考资料：
	- [Babel 7.1介绍 transform-runtime polyfill env](https://link.juejin.cn?target=https%3A%2F%2Fwww.jianshu.com%2Fp%2Fd078b5f3036a)
	- [懒加载](https://link.juejin.cn?target=http%3A%2F%2Fwebpack.docschina.org%2Fguides%2Flazy-loading%2F)
	- [Vue 路由懒加载](https://link.juejin.cn?target=https%3A%2F%2Frouter.vuejs.org%2Fzh%2Fguide%2Fadvanced%2Flazy-loading.html%23%25E8%25B7%25AF%25E7%2594%25B1%25E6%2587%2592%25E5%258A%25A0%25E8%25BD%25BD)
	- [webpack 缓存](https://link.juejin.cn?target=https%3A%2F%2Fwebpack.docschina.org%2Fguides%2Fcaching%2F)
	- [一步一步的了解webpack4的splitChunk插件](https://juejin.im/post/6844903614759043079)