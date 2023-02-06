title:: JSON.stringify

- JSON.stringify() 方法可以将一个 JavaScript 值转换为一个 [[JSON]] 字符串，实现上也是调用了 [[toString]] 方法，也算是一种类型转换的方法。下面讲一讲JSON.stringify 的注意要点： #.ol
	- 处理基本类型时，与使用[[toString]]基本相同，结果都是字符串，除了 `undefined`
		- ```js
		  console.log(JSON.stringify(null)) // null
		  console.log(JSON.stringify(undefined)) // undefined，注意这个undefined不是字符串的undefined
		  console.log(JSON.stringify(true)) // true
		  console.log(JSON.stringify(42)) // 42
		  console.log(JSON.stringify("42")) // "42"
		  ```
	- 布尔值、数字、字符串的包装对象在序列化过程中会自动转换成对应的原始值。
		- ```js
		  JSON.stringify([new Number(1), new String("false"), new Boolean(false)]); // "[1,"false",false]"
		  ```
	- `undefined`、任意的函数以及 `symbol` 值，在序列化过程中会被忽略（出现在非数组对象的属性值中时）或者被转换成 `null`（出现在数组中时）。
		- ```js
		  JSON.stringify({x: undefined, y: Object, z: Symbol("")}); 
		  // "{}"
		  
		  JSON.stringify([undefined, Object, Symbol("")]);          
		  // "[null,null,null]" 
		  ```
	- `JSON.stringify` 有第二个参数 `replacer`，它可以是数组或者函数，用来指定对象序列化过程中哪些属性应该被处理，哪些应该被排除。
		- ```js
		  function replacer(key, value) {
		    if (typeof value === "string") {
		      return undefined;
		    }
		    return value;
		  }
		  
		  var foo = {foundation: "Mozilla", model: "box", week: 45, transport: "car", month: 7};
		  var jsonString = JSON.stringify(foo, replacer);
		  
		  console.log(jsonString)
		  // {"week":45,"month":7}
		  ```
		- ```js
		  var foo = {foundation: "Mozilla", model: "box", week: 45, transport: "car", month: 7};
		  console.log(JSON.stringify(foo, ['week', 'month']));
		  // {"week":45,"month":7}
		  ```
	- 如果一个被序列化的对象拥有 `toJSON` 方法，那么该 `toJSON` 方法就会覆盖该对象默认的序列化行为：不是那个对象被序列化，而是调用 `toJSON` 方法后的返回值会被序列化，例如：
		- ```js
		  var obj = {
		    foo: 'foo',
		    toJSON: function () {
		      return 'bar';
		    }
		  };
		  JSON.stringify(obj);      // '"bar"'
		  JSON.stringify({x: obj}); // '{"x":"bar"}'
		  ```