#wip #frontend #javascript
# 原型链
1. js分为**函数对**象和**普通对象**，每个对象都有__proto__属性，但是只有函数对象才有prototype属性
2. js中每个函数对象都可以作为构造函数，其构造的对象的`__proto__`指向函数的`prototype`属性。
3. `js`中最基础的两个构造函数是`Object`和`Function`，所有函数对象的`__proto__`指向`Function.protoype`，所有普通对象的`__proto__`指向`Object.prototype`。
![[629j7gtc.bmp]]

# ES6 class

es6的class实际上只是构造函数的语法糖。

补充：super 的指向

# 参考

https://juejin.cn/post/6844903989088092174