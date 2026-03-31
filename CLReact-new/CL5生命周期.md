# 生命周期（类组件独有的概念）

```jsx
import { React } from 'react'

class App extends React.Component {
    constructor() {
        super()
    }
    
    render() {
        return (
            <div></div>
        )
    }
    
    componentDidMount() {
        
    }
    
    componentWillUnmount() {
        
    }
}

1. constructor

@1. 同一个组件对象只会创建一次，constructor只会执行一次
@2. 不可以调用setState方法

2. render

@1. 返回一个虚拟DOM,会被挂载到虚拟DOM树中，最终渲染到页面的真实DOM中
@2. render可能不只执行一次，只要重新渲染就会重新运行
@3. 严禁使用setState, 可能会导致无限递归渲染

3. componentDidMount

@1. 只会执行一次
@2. 可以使用setState
@3. 通常情况下，会将网络请求，启动计时器等一开始需要的操作放到该函数中

4. componentWillUnmount

@1. 通常在该函数中销毁一些组件依赖的资源，比如计时器

```

