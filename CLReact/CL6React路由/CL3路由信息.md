# 路由信息

## 1. v6版本之前

1. Router组件会创建一个上下文， 并且， 向上下文中注入一些信息
2. 该上下文是对开发者隐藏的， Route组件若匹配到了地址，则会将这些上下文中的信息作为属性传入对应的组件

```js
1. 所有的信息都被注入到了props中


function A(props){
    console.log(props);
}

<Route path="" component={ <A /> } />
    
    
    
    
// 1. history  他并不是window.history对象， 我们利用该对象无刷新跳转地址
    
#1. 为什么没有直接使用history对象
1. React-Router有两种模式， hash，history， 如果直接使用window.history, 只能支持一种模式
2. 当使用window.history.pushState方法时， 没有办法收到任何通知，将导致react无法知晓地址发生了变化，结果导致无法重新渲染组件

#2. 其中的方法
1. push  将某个新的地址入栈 （历史记录栈）  参数1：新的地址  参数2：可选，附带的数据

2. replace   将某个新的地址替换掉当前栈中的地址

3. go

4. forward

5. back


// 2. location 与history.location完全一致，是同一个对象，但是，与window.location不同
    
1. location对象中记录了当前地址的相关信息， query-string  用于解析地址栏中的数据

// 3. match  该对象保存了路由匹配的相关信息

1. isExact: 当前的路由和路由配置的路由是否精确匹配
2. params: 获取路径规则中对应的数据

// 4. 向某个页面传递数据时

1. 使用state， 在push页面时， 加入state

2. 利用search， 把数据填写到地址栏中?后面

3. 利用hash

4. params: 把数据填写到路径中

// 5. 非路由组件获取路由信息

某些组件，并没有直接放到Route中，而是嵌套在其他普通组件中，因此，它的props中没有路由信息，如果这些组件需要获取到路由信息，可以使用下面两种方式：

1. 将路由信息从父组件一层一层传递到子组件

2. 使用react-router提供的高阶组件withRouter，包装要使用的组件，该高阶组件会返回一个新组件，新组件将向提供的组件注入路由信息。

import { withRouter } from 'react-router-dom';

const AWrap = withRouter(A)
```



## 2. v6版本之后

1. v6版本之后就没有把信息注入到props中， 而是利用了hooks

```js
// 1. useNavigate  跳转到某个页面， 无刷新的跳转
import { useNavigate } from 'react-router'

function App(){
    const navigate = useNavigate()
    
    navigate('/a')
    navigate('/a/5')
}

// 2. useParams  获取路径上的参数

import { useParams } from 'react-router-dom'

function A(){
    const params = useParams();
    
    console.log(params)  //  params是一个对象  { id: 5 }
}


// 3. useLocation  获取路由上的信息

import { useLocation } from 'react-router'

function B(){
    const location = useLocation();
    
    console.log(location)
    
    {
        hash: '',
        key: '',
        pathname: '',
        search: '',
        state: ''
    }
}


// 4. useSearchParams   获取？后面的参数

import { useSearchParams } from 'react-router-dom'

function C(){
    const [ searchParams, setSearchParams ] = useSearchParams();
    
    // 1. 如果想获取某个参数的话
    searchParams.get('name')
    
    // 2. 想修改的话， 必须传入所有的查询参数，否则会覆盖已有参数
    setSearchParams({
        name: 'caolei'
    })
    
}
```

