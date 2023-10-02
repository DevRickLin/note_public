#wip #frontend #vue #工程化 #性能 
# Vue2
# Vue3

```js
function reactive(obj) {
  return new Proxy(obj, {
    get(target, key) {
      track(target, key)
      return target[key]
    },
    set(target, key, value) {
      target[key] = value
      trigger(target, key)
    }
  })
}

// reactive 基于对proxy的属性(key)访问做追踪，对值类型不起作用，解构后无作用，重新赋值无作用
function ref(value) {
  const refObject = {
    get value() {
      track(refObject, 'value')
      return value
    },
    set value(newValue) {
      value = newValue
      trigger(refObject, 'value')
    }
  }
  return refObject
}

// ref基于getter, setter追踪，对value重新赋值也有作用

```

https://cn.vuejs.org/guide/extras/reactivity-in-depth.html