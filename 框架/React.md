# 1. setState 同步和异步的发生场景

先说说 setState 的开销，每次 setState 都会大概经过以下过程：

1. 更新 state
2. 创建新的 vnode
3. diff 算法对比新旧 vnode tree
4. 对真实 dom tree 更新

在这个过程中，还会有 4 到 5 个组件生命周期函数会被执行：

1. shouldComponentUpdate
2. componentWillUpdate
3. render
4. componentWillReceiveProps (子组件的 props 发生改变时)
5. componentDidUpdate

所以每次更新的开销是比较大的，所以在某些情况下，先将数据缓存下来，最后统一更新是有必要的。

**各自发生的场景**

1. 异步，在 React 控制的事件监听函数和生命周期函数中调用 setState 时，使用异步更新，将 state 片段缓存在一个队列中，最后由 react 统一更新
2. 同步，在 React 控制之外的回调函数中只会同步更新，如 setTimeout，原生（addEventLisnter）绑定的事件

**React 如何控制的**

​	在 setState 的实现中，会对一个 isBatchingUpdates 进行判断，默认为 false 时进行同步更新。而在 React 接管的回调函数中会将此变量设为 true，进行异步更新

**多个setState调用会合并处理**

​	React 会将多个 this.setState 产生的修改放在一个队列里进行批延时处理

**参数为函数的setState用法**

​	对于多次调用函数式 setState 的情况，React 会保证每次调用时，state 都已经合并了之前的状态修改结果（但还未合并到 this.state上，所以后续的对象式 setState 还是使用的之前的值，尽量避免混用函数式和对象式）

