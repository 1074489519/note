- Web Worker 使用其他工作线程从而独立于主线程之外，它可以执行任务而不干扰用户界面。一个 worker 可以将消息发送到创建它的 JavaScript 代码, 通过将消息发送到该代码指定的事件处理程序（反之亦然）。
- Web Worker 适用于那些处理纯数据，或者与浏览器 UI 无关的长时间运行脚本。
- 创建一个新的 worker 很简单，指定一个脚本的 URI 来执行 worker 线程（main.js）：
	- ```js
	  var myWorker = new Worker('worker.js');
	  // 你可以通过postMessage() 方法和onmessage事件向worker发送消息。
	  first.onchange = function() {
	    myWorker.postMessage([first.value,second.value]);
	    console.log('Message posted to worker');
	  }
	  
	  second.onchange = function() {
	    myWorker.postMessage([first.value,second.value]);
	    console.log('Message posted to worker');
	  }
	  ```
- 在 worker 中接收到消息后，我们可以写一个事件处理函数代码作为响应（worker.js）：
	- ```js
	  onmessage = function(e) {
	    console.log('Message received from main script');
	    var workerResult = 'Result: ' + (e.data[0] * e.data[1]);
	    console.log('Posting message back to main script');
	    postMessage(workerResult);
	  }
	  ```
- onmessage处理函数在接收到消息后马上执行，代码中消息本身作为事件的data属性进行使用。这里我们简单的对这2个数字作乘法处理并再次使用postMessage()方法，将结果回传给主线程。
- 回到主线程，我们再次使用onmessage以响应worker回传的消息：
	- ```js
	  myWorker.onmessage = function(e) {
	    result.textContent = e.data;
	    console.log('Message received from worker');
	  }
	  ```
- 在这里我们获取消息事件的data，并且将它设置为result的textContent，所以用户可以直接看到运算的结果。
- 不过在worker内，不能直接操作DOM节点，也不能使用window对象的默认方法和属性。然而你可以使用大量window对象之下的东西，包括WebSockets，IndexedDB以及FireFox OS专用的Data Store API等数据存储机制。
- 参考资料：
	- [Web Workers](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FWeb_Workers_API%2FUsing_web_workers)