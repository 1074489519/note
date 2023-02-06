- 看规范 9.1，函数语法表示如下：
- ```js
  ToPrimitive(input[, PreferredType])
  ```
- 第一个参数是 `input`，表示要处理的输入值。
- 第二个参数是 `PreferredType`，非必填，表示希望转换成的类型，有两个值可以选，[[Number]] 或者 [[String]]。
- 当不传入 `PreferredType` 时，如果 `input` 是日期类型，相当于传入 `String`，否则，都相当于传入 `Number`。
- 如果传入的 `input` 是 [[undefined]]、[[Null]]、[[Boolean]]、[[Number]]、[[String]] 类型，直接返回该值。
- 如果是 `ToPrimitive(obj, Number)`，处理步骤如下： #.ol
	- 如果 `obj` 为 基本类型，直接返回
	- 否则，调用 [[valueOf]] 方法，如果返回一个原始值，则 JavaScript 将其返回。
	- 否则，调用 [[toString]] 方法，如果返回一个原始值，则 JavaScript 将其返回。
	- 否则，JavaScript 抛出一个类型错误异常。
- 如果是 `ToPrimitive(obj, String)`，处理步骤如下： #.ol
	- 如果 `obj` 为 基本类型，直接返回
	- 否则，调用 [[toString]] 方法，如果返回一个原始值，则 JavaScript 将其返回。
	- 否则，调用 [[valueOf]] 方法，如果返回一个原始值，则 JavaScript 将其返回。
	- 否则，JavaScript 抛出一个类型错误异常。
-