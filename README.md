## React 15生命周期方法
![生命周期](/images/React15%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)
- constructor
- componentWillReceiveProps
- componentWillMount
- componentWillUpdate
- componentDidUpdate
- componentDidMount
- render
- componentWilUnmount

> componentWillMount、componentDidMount只会在挂载阶段被调用一次，componentWillMount会在执行render方法前被触发。
render在执行过程中并不会去操作真实的DOM(也就是说不会渲染)，它的只能是把需要渲染的内容返回回来。真实DOM的渲染工作，在挂载阶段是由ReactDOM.render来承接的     
componentDidMount方法在渲染结束后被触发，此时因为真实DOM已经挂载到了页面上，可以在这个生命周期里执行真实DOM相关的操作。此外，类似于异步请求、数据初始化这样的操作也大可以放在这个生命周期来做。

## 组件初始化渲染
![初始化渲染](/images/%E6%8C%82%E8%BD%BD%E9%98%B6%E6%AE%B5.png)

### 组件的更新
![组件的更新](/images/%E7%BB%84%E4%BB%B6%E7%9A%84%E6%9B%B4%E6%96%B0.png)

- 父组件更新触发的更新
- 组件自身调用自己的setState触发的更新

> componentReceiveProps并不是由props的变化触发的，而是由父组件的更新触发的。

#### componentWillUpdate 和 componentDidUpdate
componentWillUpdate会在render前被触发，它和componentWillMount类似，允许在里面做一些不涉及真实DOM操作的准备工作；而componentDidUpdate则在组件更新完毕之后被触发，和componentDidMount类似，这个生命周期也经常被用来处理DOM事件。常常将componentDidUpdate的执行作为子组件更新完毕的标志通知父组件。

## React 16生命周期
![生命周期](/images/React16%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)
### 初始化对比
![初始化对比](/images/%E6%8C%82%E8%BD%BD%E5%AF%B9%E6%AF%94.png)
### 更新对比
![更新对比](/images/%E6%9B%B4%E6%96%B0%E5%AF%B9%E6%AF%94.png)
- 废弃了componentWillMount，新增了getDerivedStateFromProps
- 废弃componentWillUpdate，新增getSnapshotBeforeUpdate
> getSnapshotBeforeUpdate的返回值会作为第三个参数给到componentDidUpdate，执行时机是在render方法之后，真实DOM更新之前。在这个阶段里，可以同时获取到更新前的真实DOM和更新后的state&props信息。

## React15更新流程
![更新流程](/images/15%E6%9B%B4%E6%96%B0%E6%B5%81%E7%A8%8B.png)
> 同步渲染的递归调用栈是非常深的，只有最底层的调用返回了，整个渲染过程才会开始逐层返回。这个漫长切不可打断的更新过程，将会带来用户体验的巨大风险：同步渲染一旦开始，变回牢牢抓住主线程不放，直到递归彻底完成，在这个过程中，浏览器没有办法处理任何渲染之外的事情，会进入一种无法处理用户交互的状态。因此若渲染时间稍微长点，页面就会面临卡顿甚至卡死的风险。

## React16更新流程
![更新流程](/images/16%E6%9B%B4%E6%96%B0%E6%B5%81%E7%A8%8B.png)
> Fiber会将一个大的更新任务拆解为许多小任务。每当执行完一个小任务时，渲染线程都会把主线程交回去。看看有没有优先级更高的工作需要处理，确保不会出现其他任务被"饿死"的情况，进而避免同步渲染带来的卡顿。在这个过程中，渲染线程不再"一去不回头"，而是可以被打断的，这就是所谓的“异步渲染”

React16改造生命周期的主要动机是为了配合Fiber架构带来的异步渲染机制。在这个改造的过程中，针对生命周期中长期被滥用的部分推行了具有强制性的最佳实践。这一系列的工作做下来，首先是确保了Fiber机制下数据和视图的安全性，同时也确保了生命周期方法的行为更加纯粹、可控、可预测。