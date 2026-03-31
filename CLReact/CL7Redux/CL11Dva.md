# DVA

## 1. 基本概念

1. dva不仅仅是一个第三方库，更是一个框架， 它主要整合了redux的相关内容，让我们处理数据更加方便， 实际上，dva依赖了很多，react， react-router， redux， redux-saga,  react-redux, connected-react-router

## 2. 基本使用

1. dva默认导出一个函数，通过调用该函数，可以得到一个dva对象

2. **dva对象.router**：路由方法，传入一个函数，该函数返回一个react节点，当应用程序启动之后，会自动渲染该节点

3. **dva对象.start**：该方法用于启动dva应用程序，可以认为启动的就是react程序， 该函数传入一个选择器， 用于选中页面中的某个dom元素， react会将内容渲染到该元素内部

4. **dva对象.model**：该方法用于定义一个模型，该模型可以理解为redux的action， reducer， redux-saga的副作用处理的整合，整合成一个对象，将对象传入到model方法中

   #1. namespace：命名空间，该属性是一个字符串，字符串的值，会被作为仓库中的属性保存

   #2. state： 该模型的默认状态

   #3. reducers：该属性配置为一个对象，对象中的每一个方法就是一个reducer，dva约定方法的名字就是匹配的action的类型

   #4. effects：处理副作用的，底层就是使用redux-saga实现的，该属性配置为一个对象，对象中的每一个方法均会处理副作用，方法的名称就是一个action， 有两个参数

   第一个为action， 第二个为封装好的saga/effects对象

   #5. subscriptions：配置为一个对象，该对象中可以写任意数量任意名称的属性，每一个属性是一个函数，这些函数会在模型加入到仓库后立即运行， 所以可以用来做监听



```js
// index.js

import App from ''
import createDva from 'dva';
import countModel from ''

const app = createDva();

app.router(() => <App />)
           
//定义模型，必须要在启动程序之前定义
app.model(countModel);   
        
// 启动react程序
app.start('#root');

```

```js
// count.js

export default {
    namespace: 'count',
    state: 0,
    reducers: {
        add(state){
            return state + 1
        },
        sub(state){
            return state - 1
        },
        set(state, { payload }){
            return state + payload
        }
    },
    effects: {
        *asyncAdd(){
            
        }
    }
}
```



## 3. 和路由连用

1. 在dva中同步路由到仓库，再调用dva函数的时候， 需要配置history对象， 在使用Connectedrouter组件提供路由上下文
2. **最终的目的就是保持history一样**

```js
// 在dva中

import { createBrowserHistory } from 'history';
import createDva from 'dva';

// 第一步
const app = createDva({
    history: createBrowserHistory()
});

app.router(router);

// 第二步
import { reduxRouter, Switch, NavLink, Route } from 'dva/router';

//会往这个props中注入一个history
export default function(props){
    return (
        <reduxRouter.ConnectedRouter history={ props.history } >
        
        </reduxRouter.ConnectedRouter>
    )
}
```



## 4. dva的配置

1. **history**：同步到仓库的history对象
2. initialState：创建redux仓库时，使用的默认状态
3. onError：当仓库的运行发生错误时，运行的函数
4. **onAction**：可以配置redux中间件， 传入一个中间件，或者中间件数组
5. onStateChange：当仓库中的状态发生变化时运行的函数
6. onReducer：对模型中的reducer进一步封装
7. onEffect：类似于对模型中的effect的进一步封装
8. extrareducers：用于配置额外的reducer，他是一个对象，对象的每一个属性是一个方法，每个方法就是一个需要合并的reducer，方法名即属性名
9. extraEnhancers：他是用来封装createStore函数的，dva会将原来的仓库创建函数作为参数传递，返回一个新的用于创建仓库的函数，函数必须放置到数组中



## 5. dva插件

1. 通过dva对象.use(插件)，来使用插件，插件本质上就是一个对象，该对象与配置对象相同，在dva启动时，将传递的插件对象混合到配置中

```js
const logger = {
    
}

export default {
    onAction: logger
}

app.use(chajian)
```



#### dva-loading

1. 该插件会在仓库中加入一个状态， 名称为loading， 他是一个对象，有如下属性
2. gobal: 全局是否正在处理副作用， 只要有任何一个模型在处理副作用，则该属性为true
3. model: 一个对象，对象中的属性名以及属性的值，表示哪一个对应的模型是否在处理副作用
4. effects:一个对象，对象中的属性名以及属性的值，表示的是哪一个action触发了副作用