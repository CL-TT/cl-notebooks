# Ref和Ref转发

## 1. 为什么要使用ref

1. 可以作用到真实的Dom上，来获取Dom的相关属性和方法
2. 可以作用到类组件上，获取到这个类的实例
3. 不可以在函数组件上使用ref,   但是可以在函数组件里面使用



## 2. 如何使用ref

1.在类组件中

```jsx
1. divRef是一个对象  { current: '' }
2. this.divRef.current    // 使用方式


class APP extends React.component {
    constructor() {
       this.divRef = React.createRef()
    }
    
    render() {
        return (
          <div ref={ this.divRef }></div>
        )
    }
}
```

2.函数组件中

```jsx
两种方式React.createRef和hook，useRef

import { useRef } from 'react'

function App() {
    const divRef = useRef()
    
    return (
        <div ref={ divRef }></div>
    )
}
```



## 3. 为什么会有Ref转发

1. **ref转发，是为了让父组件能够能够拿到子组件内部的Dom， 函数组件和类组件都可以使用ref转发**

2. 因为ref直接作用到类组件上拿到的是这个类实例，而作用到函数组件上是完全没作用的就直接报错
3. React.forwardRef()  它会传入一个组件，返回一个新的组件，然后父组件的ref会化身在在原来的组件上的第二个参数
4. 如果想要获取到子组件（函数组件）里面的方法，就可以使用useImperativeHandle， 类组件直接用实例调用就可以

```jsx
import { useRef, useImperativeHandle } from 'react'

function App() {
    const ref = useRef()
    
    return (
      <div>
        <NewComp ref={ ref }/>  
      </div>
    )
}

// 那么这个ref就是父组件传入的ref, 这样父组件就可以拿到子组件的Dom元素
function Comp(props, ref) {
    useImperativeHandle(ref, () => ({
        click: () => {
            console.log('子组件暴露出的方法')
        }
    }))
    
    return (
      <div ref={ ref }></div>
    )
}

export React.forwardRef(Comp)


class Comp extends React.component {
    render() {
        return (
          <div ref={ this.props.forwardref }></div>
        )
    }
}

export React.forwardRef((props, ref) => {
    return <Comp forwardref={ ref } { ...props } />
})
```

