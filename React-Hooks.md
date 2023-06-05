## 类组件(Class Component)
> 就是基于ES6 Class这种写法，通过继承React.Component得来的React组件
```js
class DemoClass extends React.Component {
    state = {
        text: ''
    }

    componentDidMount() {}

    changeText = (newText => {
        this.setState({ text: newText })
    })

    render() {
        return <div className='demo'>
            <p>{this.state.text}</p>
            <button onClick={this.changeText}>修改</button>
        </div>
    }
}
```

## 函数组件/无状态组件(Function Component / Stateless Component)
> 以函数的形态存在的React组件，函数组件内部无法定义和维护state，因此它还有一个别名叫“无状态组件”

```js
function DemoFunction(props) {
    const { text } = props

    return <div className='demo'>
        <p>{`function 组件所接受到的来自外界的文本内容是：[${text}]`}</p>
    </div>
}
```

### 函数组件与类组件的对比
- 类组件需要继承class，函数组件不需要- 类组件可以访问生命周期方法，函数组件不能
- 类组件中可以获取到实例化后的this，并基于这个this做各种各样的事情，函数组件不可以
- 类组件中可以定义维护state(状态)，而函数组件不可以