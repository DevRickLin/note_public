#react #frontend #工程化 #wip 
## MVC的问题
![[Pasted image 20230305130615.png]]
根据[Facebook 8年前提出Flux架构的视频](https://www.youtube.com/watch?v=nYkdrAPrdcw&t=250s)，最初FaceBook在处理消息Thread(或许可以翻译成对话群组)的未读计数与未读高亮显示时遇到了复杂状态处理的问题。原文中提到，当标记一个消息Thread为已读，需要更新整个消息Thread的Model，同时还需要更新未读标记的Model，尝试展开整个流程后，我们会发现这里面的依赖关系的确很复杂:
1. 用户标记消息Thread为已读 view -> ThreadModel 
2. ThreadModel更新后，根据ThreaModel信息，更新未读计数 ThreadModel -> UnreadCountModel
3. ThreadModel更新后，更新高亮 ThreadModel -> view
4. UnreadCountModel更新后, 更新未读计数显示 UnreadCountModel -> view
![[Pasted image 20230305133313.png]]
上面的示意图来自原视频。我们可以看到其中`Model`和`View`存在$n$对$n$的关系，而且`Model`之间还可能会存在需要我去获取`Model A`的数据，带到`Model B`去这种关系。
更广泛地说，这种混乱的关系来自于数据流向的不确定性。不同业务可能会创造出无数从不同方向，往不同地方去的数据流，使得逻辑变的十分不清晰。

## Flux的架构
Flux的基本心智模型是单向数据流(`unidirectional data flow`)。

`View`建立`Action`，(`Action`可以当作是用户行为的抽象)，`Action`通过`Dispatcher`被派发到一个**单一**的`Dispatcher`，`Dispatcher`将`Action`经由回调函数传给所有的`Store`，由`Store`自己决定要如何回应，并提交`change`事件给`Controller-views`。`Controller-views`是一种特殊的`view`，其监听`change`事件，从`Store`获取数据并触发自身及子孙`view`的状态更新。

相对于`MVC`模型，其数据的流向是十分明确的，所有用户行为导致的数据传递都会汇聚到`Dispatcher`，并且所有的`Store`的控制是被**反转**的，`Dispatcher`不会控制，调用`Store`中的任何方法，从而也不需要关心`Store`的实现细节，而`Store`可以自己根据数据决定怎么更新数据模型。

在这个心智模型中没有提到的是，`Store`到`View`的数据流也发生了控制**反转**,`Store`并不会通过方法调用或其他手段去更新视图，也不关心需要传递什么给视图，而是通过事件通知`Controller-view`更新，由`Controller-view`通过store提供的公开接口，自己决定要取得哪些数据，并且**单向**的向下传递更新。

***单向数据流加上控制反转，解决了数据流向复杂的问题，也解耦了`Flux`中的各个模块，数据流的上游不再需要关心下游的细节, 下游的更改也不再会扰动上游的内部实现。***

![[Pasted image 20230305135044.png]]
![[Pasted image 20230305135603.png]]

## 为何不双向绑定(Two-Way Binding)

除了`Flux`架构，`MVVM`架构作为`Vue`框架的核心思想，提出了双向绑定这个对于复杂数据流的解决方案，其基本思路是通过框架自身的能力
1. 在数据变化后更新视图
2. 在视图变化后更新数据。
从而掩盖了数据和视图中复杂的关联细节。

对于为何不采用双向绑定，`Flux`原话是这样说的:
	We found that two-way data bindings led to cascading updates, where changing one object led to another object changing, which could also trigger more updates. As applications grew, these cascading updates made it very difficult to predict what would change as the result of one user interaction. When updates can only change data within a single round, the system as a whole becomes more predictable.

翻译：
	我們發現雙向數據綁定會導致級聯更新，其中更改一個對象會導致另一個對象更改，這也可能觸發更多更新。隨著應用程序的增長，這些級聯更新使得預測一次用戶交互會導致什麼變化變得非常困難。當更新只能在一輪內更改數據時，整個系統變得更可預測。

也就是说，双向绑定主要的问题在于其不可预测性。以`Vue`为例，当我们更新一个响应式数据时，`Vue`对视图所作的一切对开发者来说是个黑盒子，使得某些问题没办法直观的发现，需要你更深入的了解`Vue`的实现，或是好好遵守`Best Practice`来避免。相较之下，`React`就简单很多，无论如何，只要`state`更新就重新触发`render`。

不过同样的，不采用双向绑定的`Flux`架构将这部分的复杂性展开给了开发者，这也是`Vue`和`React`当前两大主流框架有着不同`Trade-off`选择的主要分歧。

# 参考
https://facebook.github.io/flux/

