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

## Redux
> Redux是Javascript状态容器，他可以提供可预测的状态管理

假如把React项目里面的所有组件拉一个钉钉群，nameRedux就充当了这个群里的“群文件”角色，所有的组件都可以吧需要在组件树里流动的数据存储在群文件里。当某个数据改变的时候，其他组件都能够通过下载最新的群文件来获取到数据的最新值。这就是“状态容器”的含义——存放公共数据的仓库。

- Redux的组成
    - store
    - reducer
    - action

store就好比组建群里的“群文件”，它是一个**单一的数据源**，而且是只读的       
action“动作”的意思，它是**对变化的描述**        
reducer是一个函数，负责“对变化进行分发和处理”，最终将新的数据返回给store
![redux工作流](/images/reducx%E5%B7%A5%E4%BD%9C%E6%B5%81.png)

如果相对数据进行修改，只有一种途径：派发action。action会被reducer读取，进而根据action内容的不同对数据进行修改、生成新的state(状态)，这个新的state会更新到store对象里，进而驱动试图层面做出对应的改变。
- 使用createStore来完成store对象的创建
- reducer的作用是将新的state返回给state
    - 一个reducer一定是一个纯函数，可以有各种各样的内在逻辑，但最终一定要返回一个state
- action的作用是通知reducer“让改变发生”
- 派发action，靠的是dispatch

createStore方法是一切的开始，它接受三个入参：
- reducer
- 初始状态内容
- 指定中间件
![redux流程图](/images/redux%E6%B5%81%E7%A8%8B%E5%9B%BE.png)