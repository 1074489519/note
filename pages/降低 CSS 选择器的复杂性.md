- ### (1). 浏览器读取选择器，遵循的原则是从选择器的右边到左边读取。
	- 看个示例
		- ```
		  #block .text p {
		  	color: red;
		  }
		  ```
		- 1. 查找所有 P 元素。
		- 2. 查找结果 1 中的元素是否有类名为 text 的父元素
		- 3. 查找结果 2 中的元素是否有 id 为 block 的父元素
- ### (2). CSS 选择器优先级
	- ```
	  内联 > ID选择器 > 类选择器 > 标签选择器
	  ```
- 根据以上两个信息可以得出结论。
	- 1. 选择器越短越好。
	- 2. 尽量使用高优先级的选择器，例如 ID 和类选择器。
	- 3. 避免使用通配符 *。
- 最后要说一句，据我查找的资料所得，CSS 选择器没有优化的必要，因为最慢和慢快的选择器性能差别非常小。
- 参考资料：
- [CSS selector performance](https://link.juejin.cn?target=https%3A%2F%2Fecss.io%2Fappendix1.html)
- [Optimizing CSS: ID Selectors and Other Myths](https://link.juejin.cn?target=https%3A%2F%2Fwww.sitepoint.com%2Foptimizing-css-id-selectors-and-other-myths%2F)