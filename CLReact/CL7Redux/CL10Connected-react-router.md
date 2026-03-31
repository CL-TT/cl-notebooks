# connected-react-router

## 1. redux调试工具

1. chrome插件：redux-devtools
2. 使用npm安装第三方库：redux-devtools-extenstion



## 2. 基本使用

### 1. 在合并reducer的时候加入一个reducer

1. connectRouter函数， 传入一个history对象， 得到一个reducer

```js
import { combineReducers } from 'redux';
import { connectRouter } from 'connected-react-router';
import { createBrowserHistory } from 'history';

export default combinereducers({
    count,
    user,
    router: connectRouter(createBrowserhistory())
})
```



### 2. 加入一个中间件

1. routerMiddlewear函数，传入history对象，得到一个redux中间件

```js
import { createStore, applyMiddlewear } from 'redux';
import { routerMiddlewear } from 'connected-react-router';
import { createBrowserHistory } from 'history';

const routerMidwear = routerMiddlewear(createBrowserHistory());

const store = createStore(reducer, applyMiddlewear(routerMidwear))
```



### 3. 使用ConnectedRouter组件

1. ConnectedRouter组件，传入history对象，提供路由上下文

```js
import { ConnectRouter } from 'connected-react-router';
import { createBrowserHistory } from 'history';

export default function(){
    return (
        <Connectedrouter history={ createBrowserHistory() } />
    )
}
```



### 4. 使用编程式跳转

```js
import { push } from 'connected-react-router';

// 这个push函数是一个action创建函数

function Test(props){
    return (
        <button onClick={  }>跳转</button>
    )
}

function mapFun(dispatch){
    return {
        go(){
            dispatch(push());
        }
    }
}


// 两种方式

//1. 使用props中注入的history
props.history.push()

//2. 使用connect连接，来触发action
```

