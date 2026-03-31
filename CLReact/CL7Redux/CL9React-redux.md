# React-redux(连接react和redux的库), 以及Redux Toolkit

## 1. 基本概念以及使用

1. Redux是一个独立的第三方库， 之后React官方在Redux基础上推出了React-redux

2. Provider组件： 没有任何的ui界面， 该组件的作用， 是将redux的仓库放到一个上下文中

3. connect组件：它返回一个高阶组件， 用于连接组件， 第一个参数是要连接的属性， 第二个参数是要连接的方法

```js
import { Provider, connect } from 'react-redux';

function App(){
    return (
        <Provider store={ store }></Provider>
    )
}


function Test(){
    return()
}


export default connect(mapProp, mapFun)(Test)
```



## 2. 安装

npm i  @reduxjs/toolkit  react-redux