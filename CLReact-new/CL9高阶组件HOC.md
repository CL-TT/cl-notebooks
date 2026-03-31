# 高阶组件HOC

## 1. 什么是高阶组件

高阶组件是一个函数，会传入一个组件作为参数，然后返回一个新的组件

```jsx
function withLog(Comp) {
    return function NewComp(props) {
        // 就可以写公共逻辑
        
        return <Comp { ...props }/>
    }
}
```



## 2. 高阶组件解决什么样的问题

在以前类组件为核心的时候，高阶组件是抽离类组件中的公共逻辑（当然也可以抽离函数组件中的），它是进行横切关注点的，那么在hook出来后，组件的核心慢慢偏向于函数组件，那么自定义hook也可以完成这个抽离公共逻辑的任务

```jsx
一个记录组件日志的高阶组件

import { useEffect } from 'react'

export default withLog(Comp) {
    return function NewComp(props){
        useEffect(() => {
            console.log(`组件${Comp.name}创建了`)
        }, [])
        
        return <Comp { ...props }/>
    }
}

const NewComp = withLog(Comp)
```

