#wip #react #frontend
-   **我们在 `jsx` 中绑定的事件(demo中的`handerClick`，`handerChange`),根本就没有注册到真实的`dom`上。是绑定在`document`上统一管理的。**
-   **真实的`dom`上的`click`事件被单独处理,已经被`react`底层替换成空函数。**
-   **我们在`react`绑定的事件,比如`onChange`，在`document`上，可能有多个事件与之对应。**
-    **`react`并不是一开始，把所有的事件都绑定在`document`上，而是采取了一种按需绑定，比如发现了`onClick`事件,再去绑定`document click`事件。**

-   原生事件（阻止冒泡）会阻止合成事件的执行
    
-   合成事件（阻止冒泡）不会阻止原生事件的执行
# 事件池

https://juejin.cn/post/6955636911214067720
# 参考
https://juejin.cn/post/7068649069610024974