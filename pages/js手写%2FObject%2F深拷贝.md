title:: js手写/Object/深拷贝

- ```js
  const mapTag = "[object Map]"
  const setTag = "[object Set]"
  const arrayTag = "[object Array]"
  const objectTag = "[object Object]"
  
  const boolTag = "[object Boolean]"
  const dateTag = "[object Date]"
  const errorTag = "[object Error]"
  const numberTag = "[object Number]"
  const regexpTag = "[object RegExp]"
  const stringTag = "[object String]"
  const symbolTag = "[object Symbol]"
  
  const deepTag = [mapTag, setTag, arrayTag, objectTag]
  
  function getType(target) {
    return Object.prototype.toString.call(target)
  }
  
  function isObject(target) {
    const type = typeof target
    return target !== null && (type === "object" || type === "function")
  }
  
  function getInit(target) {
    const Ctor = target.constructor
    return new Ctor()
  }
  
  function forEach(array, iterate) {
    let index = -1
    const length = array.length
    while (++index < length) {
      iterate(array[index], index)
    }
    return array
  }
  
  function clone(target, map = new WeakMap()) {
    // 克隆原始类型
    if (!isObject(target)) {
      return target
    }
  
    const type = getType(target)
    let cloneTarget
    if (deepTag.includes(type)) {
      cloneTarget = getInit(target)
    }
  
    if (map.has(target)) {
      return target
    }
    map.set(target, cloneTarget)
  
    // 克隆set
    if (type === setTag) {
      target.forEach((value) => {
        cloneTarget.add(close(value))
      })
      return cloneTarget
    }
    // 克隆map
    if (type === mapTag) {
      target.forEach((value, key) => {
        cloneTarget.set(key, clone(value))
      })
    }
    // 克隆对象和数组
    const keys = type === arrayTag ? undefined : Object.keys(target)
    forEach(keys || target, (value, key) => {
      if (keys) key = value
      cloneTarget[key] = clone(target[key], map)
    })
    return cloneTarget
  }
  ```