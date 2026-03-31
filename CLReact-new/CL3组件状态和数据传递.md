# 组件状态和数据传递

## 1. 组件状态 state

就是组件自己的状态数据

```jsx
1. 类组件中的state

1.1 要想改变state中的值，就必须使用setState
1.2 setState可能是异步的，所以永不不要信任setState更改的值
1.3 可以通过setState的第二个参数获取，设置之后的值

export default class App extends React.Component {
    constructor() {
        super() {
            
        }
        
        this.state = {
            name: '哈哈',
            num: 1
        }
    }
    
    // 改变数字
    changeNum() {
        this.setState({
            num: this.state.num + 1
        }, () => {
            console.log('num', this.state.num)
        })
    }
    
    render() {
        return (
            <div className="state-wrap">
              <div>数字：{ this.state.num }</div>
              <button onClick={ () => this.changeNum }>改变数字</button>
            </div>
        )
    }
}
```



## 2. 数据传递 props

1. props.children 可以实现插槽功能
1. 属性校验需要安装prop-types    npm i prop-types

```jsx
// 子组件, 可以使用父组件传递过来的数据
import PropTypes from 'prop-types'

function Hello(props) {
    const changeInfo = () => {
        props.changeInfo()
    }
    return (
        <div>
            <div>姓名：{ props.info.name }</div>
            <div>年龄：{ props.info.age }</div>
            
            <div onClick={ changeInfo }>改变父组件的参数</div>
        </div>
    )
}

// 属性默认值
Hello.defaultProps = {
    info: {
        name: 'kk',
        age: 100
    }
}

// 校验属性
Hello.propTypes = {
    info: PropTypes.object.isRequired
}

// 父组件
function App() {
    return (
        <div>
            <Hello info={ { name: 'caolei', age: 20 } } changeInfo={ changeInfo } />
        </div>
    )
}
```

