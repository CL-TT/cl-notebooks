# Redux中间件

## 1. 中间件的概念

1. 中间件类似于插件， 可以在不影响原本功能，并且不改动原本代码的基础上，对其功能进行增强， 在Redux中， **中间件主要是增强dispatch函数**
2. **实现Redux中间件的基本原理，就是更改仓库中的dispatch函数**



## 2. 中间件的书写

1. 中间件本身就是一个函数，该函数接受一个store参数， 表示创建的仓库，该仓库并非一个完整的仓库对象，仅包含getState, dispatch, 该函数运行的时间是在仓库创建之后运行。
2. 要使用的话，需要借助applyMiddleware

```js
function log({ getState, dispatch }){
    return function(nextDispatch){
        return function dispatch(action){
            
        }
    }
}

const log2 = store => nextDispatch => action => {
    
}

const log3 = store => nextDispatch => action => {
    
}

import { createStore, applyMiddlewear } from 'redux'

const store = createStore(reducer, applyMiddlewear(log1, log2, log3))

// 理解： 

1. store这个参数，是原有的仓库仓库里面的dispatch，会先交给log3, log3返回的dispatch， 再交给log2,  log2返回的dispatch再交给log1,  然后log1返回的dispatch再替换掉原有的store里面的dispatch
```

