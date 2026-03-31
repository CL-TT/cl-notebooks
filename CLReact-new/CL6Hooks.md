# Hooks

1. Hook是react16.8的新增特性，可以在不写class的情况下使用state以及其他的React特性

## 1. Hooks出现解决的问题

1. 告别让人疑惑的生命周期 （可能在不同的生命周期中使用了相同的代码）
2. 告别类组件中烦人的this
3. 告别繁重的类组件，回归到前端人员更熟悉的函数
4. **最重要的是，React思想上的转变，从面向对象的思想开始转向为函数式编程的思想**



## 2. 使用hooks需要注意的问题

1. 只能在函数最外层调用hook, **不要在循环，条件判断或者子函数中调用**
2. **只能在React函数组件中调用hook**, 不要在其他的javascript函数中调用



## 3. 具体的Hooks使用

### 1. useState

使用状态数据的钩子

```jsx
import { useState } from 'react'

function app {
    const [count, setCount] = useState(0)
    
    const handleAdd = () => {
        setCount(++count)
    }
    return (
      <div>
        <div>{ count }</div>
        <div onClick={ handleAdd }>加一</div>
      </div>
    )
}
```



### 2. useEffect

处理副作用的钩子函数

1. 它有两个参数，第一个参数是一个回调函数，专门用于处理副作用的， 第二个参数是依赖项，依赖的数据发生变化，那么这个副作用处理函数就会重新执行一次
2. 这个处理函数有一个返回值，返回的必须是一个函数，这个函数被称为清理函数，清理函数的执行是在处理函数之前的，但是第一次他不会执行

```jsx
import { useEffect } from 'react'

function app() {
    useEffect(() => {
        return () => {}
    }, [])
   
    return (
      <div>
        
      </div>
    )
}
```

