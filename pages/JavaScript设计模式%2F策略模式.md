title:: JavaScript设计模式/策略模式

- 定义
	- 定义一系列的算法，把他们一个个封装起来，并且使他们可以相互替换
	- 策略模式的目的就是将算法的使用和实现分离开来
-
	- 一个基于策略模式的程序至少由两部分组成。第一个部分是一组策略类（可变），策略类封装了具体的算法，并负责具体的计算过程。第二个部分是环境类Context（不变），Context接受客户的请求，随后将请求委托给某一个策略类。要做到这一点，说明Context中要维持对某个策略对象的引用。
-
- 实现
	- ```js
	  /* 策略类 */
	  var levelOBJ = {
	    A: function (momey) {
	      return momey * 4
	    },
	    B: function (momey) {
	      return momey * 3
	    },
	    C: function (momey) {
	      return momey * 2
	    },
	  }
	  
	  /* 环境类 */
	  var calculateBouns = function (level, momey) {
	    return levelOBJ[level](momey)
	  }
	  console.log(calculateBouns("A", 10000)) // 40000
	  ```