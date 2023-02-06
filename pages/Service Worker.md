- 概述
	- Service workers 本质上充当 Web 应用程序、浏览器与网络（可用时）之间的代理服务器。这个 API 旨在创建有效的离线体验，它会拦截网络请求并根据网络是否可用来采取适当的动作、更新来自服务器的的资源。它还提供入口以推送通知和访问后台同步 API。
	- > 一句话概括：一个服务器与浏览器之间的中间人角色，如果网站中注册了`service worker`那么它可以拦截当前网站所有的请求，进行判断（需要编写相应的判断程序），如果需要向服务器发起请求的就转给服务器，如果可以直接使用缓存的就直接返回缓存不再转给服务器。从而大大提高浏览器体验。
- 描述
	- 基于`Web Worker`（一个独立于JavaScript主线程的独立线程，在里面执行需要消耗大量资源的操作不会堵塞主线程）
	- 在`Web Worker`的基础上增加了离线缓存的能力
	- 本质充当Web应用程序（服务器）与浏览器之间的代理服务器（可以拦截全站的请求，并作出相应的动作 -> 由开发者指定的动作）
	- 创建有效的离线体验（将一些不常更新的内容缓存在浏览器，提高访问体验）
	- 由事件驱动的，具有生命周期
	- 可以访问`cache`和`indexDB`
	- 支持推送
	- 并且可以让开发者自己控制管理缓存的内容以及版本
- 使用 #.ol
	- 注册`Service Worker`在你的`index.html`加入以下内容
		- ```js
		  /* 判断当前浏览器是否支持serviceWorker */
		  if ('serviceWorker' in navigator) {
		    /* 当页面加载完成就创建一个serviceWorker */
		    window.addEventListener('load', function () {
		      /* 创建并指定对应的执行内容 */
		      /* scope 参数是可选的，可以用来指定你想让 service worker 控制的内容的子目录。 在这个例子里，我们指定了 '/'，表示 根网域下的所有内容。这也是默认值。 */
		      navigator.serviceWorker.register('./serviceWorker.js', {scope: './'})
		        .then(function (registration) {
		  
		        console.log('ServiceWorker registration successful with scope: ', registration.scope);
		      })
		        .catch(function (err) {
		  
		        console.log('ServiceWorker registration failed: ', err);
		      });
		    });
		  }
		  ```
	- 安装`worker`：在指定的处理程序`serviceWorker.js`中书写对应的安装及拦截逻辑
		- ```js
		  /* 监听安装事件，install 事件一般是被用来设置你的浏览器的离线缓存逻辑 */
		  this.addEventListener('install', function (event) {
		   	
		      /* 通过这个方法可以防止缓存未完成，就关闭serviceWorker */
		      event.waitUntil(
		          /* 创建一个名叫V1的缓存版本 */
		          caches.open('v1').then(function (cache) {
		              /* 指定要缓存的内容，地址为相对于跟域名的访问路径 */
		              return cache.addAll([
		                  './index.html'
		              ]);
		          })
		      );
		  });
		  
		  /* 注册fetch事件，拦截全站的请求 */
		  this.addEventListener('fetch', function(event) {
		    event.respondWith(
		      // magic goes here
		        
		        /* 在缓存中匹配对应请求资源直接返回 */
		      caches.match(event.request)
		    );
		  });
		  ```
- 注意事项
	- `Service worker`运行在worker上下文，所以不能访问DOM
	- 他设计完全异步，同步API（如XHR和localStore）不能在`Service worker`中使用
	- 出于安全考量，`Service worker`只能由HTTPS承载
	- 在`Firefox`浏览器的用户隐私模式，`Service Worker`不可用
	- 其生命周期与页面无关（关联页面未关闭时，它也可以退出，没有关联页面时，它也可以启动）
- 参考
	- [网易云课堂 Service Worker 运用与实践](https://mp.weixin.qq.com/s/3Ep5pJULvP7WHJvVJNDV-g)
	-
-