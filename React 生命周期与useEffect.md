#wip #react #frontend 
# React class组件生命周期

![[b5fa490ee4d948dba86a950bfe08dede~tplv-k3u1fbpfcp-zoom-in-crop-mark 4536 0 0 0 1.webp]]
# 重点

1. render阶段
	1. contructor, getDerivedStateFromProps 初始化
	3. shouldComponentUpdate做渲染优化, 无法访问this, 不应有副作用
	2. UNSAFE_componentWillMount, UNSAFE-componentWillUpdate ，通常用于引入副作用，修改state不会触发额外渲染，在加入fiber后可能会多次调用，不建议使用
	5. 调用render
2. commit阶段
	1. getSnapshotBeforeUpdate, 在dom更新前调用，可以访问dom，接收上一个状态的state和props。
	2. componentDidMount, componentDidUpdate, 在组件挂载后执行，可以访问state，推荐进行请求及添加订阅（不怕因为fiber而反复调用）
	3. componentWillUnmount, 在组件卸载时调用

# 参考
[React 生命周期详解](https://juejin.cn/post/7096137407409422344)
[使用Effect Hook](https://zh-hans.reactjs.org/docs/hooks-effect.html)