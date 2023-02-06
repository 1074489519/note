title:: JavaScript设计模式/单例模式

- 适用场景：一个单一对象。比如：弹窗，无论点击多少次，弹窗只应该被创建一次。
- ```js
  class CreateUser {
    constructor(name) {
      this.name = name
      this.getName()
    }
    getName() {
      return this.name
    }
  }
  // 代理实现单例模式
  var ProxtMode = (function() {
    var instance = null
    return function(name) {
      if(!instance) {
        instance = new CreateUser(name)
      }
      return instance 
    }
  })()
  // 测试
  var a = new ProxyMode("aaa")
  var b = new ProxyMode("bbb")
  // 因为单例模式只实例化一次，所以下面的实例是相等的
  console.log(a === b) // true
  ```