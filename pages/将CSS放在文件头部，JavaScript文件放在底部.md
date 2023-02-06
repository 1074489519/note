- CSS 执行会阻塞渲染，阻止 JS 执行
- JS 加载和执行会阻塞 HTML 解析，阻止 CSSOM 构建
- 如果这些 CSS、JS 标签放在 HEAD 标签里，并且需要加载和解析很久的话，那么页面就空白了。所以 JS 文件要放在底部（不阻止 DOM 解析，但会阻塞渲染），等 HTML 解析完了再加载 JS 文件，尽早向用户呈现页面的内容。
  
  那为什么 CSS 文件还要放在头部呢？
  
  因为先加载 HTML 再加载 CSS，会让用户第一时间看到的页面是没有样式的、“丑陋”的，为了避免这种情况发生，就要将 CSS 文件放在头部了。
  
  另外，JS 文件也不是不可以放在头部，只要给 script 标签加上 defer 属性就可以了，异步下载，延迟执行。
  
  参考资料：
- [使用 JavaScript 添加交互](https://link.juejin.cn?target=https%3A%2F%2Fdevelopers.google.com%2Fweb%2Ffundamentals%2Fperformance%2Fcritical-rendering-path%2Fadding-interactivity-with-javascript)