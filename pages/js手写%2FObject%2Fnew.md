title:: js手写/Object/new

- ## 介绍
	- 文档：[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)
	- > new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象类型之一
- ## 用法
  id:: 6310cd72-adfa-4ecf-93cf-657e37867789
	- ```js
	  // Otaku 御宅族，简称宅
	  function Otaku (name, age) {
	      this.name = name;
	      this.age = age;
	  
	      this.habit = 'Games';
	  }
	  // 因为缺乏锻炼的缘故，身体强度让人担忧
	  Otaku.prototype.strength = 60;
	  Otaku.prototype.sayYourName = function () {
	      console.log('I am ' + this.name);
	  }
	  var person = new Otaku('Kevin', '18');
	  console.log(person.name) // Kevin
	  console.log(person.habit) // Games
	  console.log(person.strength) // 60
	  person.sayYourName(); // I am Kevin
	  ```
	- 从这个例子中，我们可以看到，实例 person 可以： #.ol
		- 访问到 `Otaku` 构造函数里的属性
		- 访问到 `Otaku.prototype` 中的属性
	- 因为 `new`是关键字，所以无法像`bind`  函数一样直接覆盖，所以我们写一个函数，命名为 `objectFactory`，来模拟 `new` 的效果。用的时候是这样的：
	- ```js
	  function Otaku () {
	      ……
	  }
	  
	  // 使用 new
	  var person = new Otaku(……);
	  // 使用 objectFactory
	  var person = objectFactory(Otaku, ……)
	  ```
- ## 实现
	- 思路步骤，： #.ol
		- 用`new Object()` 的方式新建了一个对象 `obj`
		- 设置原型，将对象的原型设置为函数的 prototype 对象。
		- 让函数的 this 指向这个对象，执行构造函数的代码（为这个新对象添加属性）
		- 判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，就返回这个引用类型的对象。
	- 代码
		- ```js
		  function objectFactory() {
		    let newObject = null;
		    let constructor = Array.prototype.shift.call(arguments);
		    let result = null;
		    // 判断参数是否是一个函数
		    if (typeof constructor !== "function") {
		      console.error("type error");
		      return;
		    }
		    // 新建一个空对象，对象的原型为构造函数的 prototype 对象
		    newObject = Object.create(constructor.prototype);
		    // 将 this 指向新建对象，并执行函数
		    result = constructor.apply(newObject, arguments);
		    // 判断返回对象
		    let flag = result && (typeof result === "object" || typeof result === "function");
		    // 判断返回结果
		    return flag ? result : newObject;
		  }
		  // 使用方法
		  objectFactory(构造函数, 初始化参数);
		  
		  ```
-