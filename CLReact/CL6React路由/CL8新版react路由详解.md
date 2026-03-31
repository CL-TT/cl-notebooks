# 新版react路由详解

## 1. v6版本之后就开始使用react-router-dom

```react
1. npm install react-router-dom

2. ***********基本的路由实现************

Router一定是放在最顶层的, BrowserRouter是history模式

import { BrowserRouter as Router, Routes, Route } from 'react-router-dom'

function App(){
    return (
        <Router>
          <Routes>
            <Route path="" element={}></Route>
            <Route path="" element={}>
               <Route path="" element={}></Route>=
            </Route>
          </Routes>
        </Router>
    )
}

3. *********嵌套路由书写在Route组件的内部， 实现对路由的定义， 出口用Outlet来显示匹配到的子组件， 有点类似于angular中的路由**********

#1. 在嵌套路由中，如果url仅匹配了父级url， 则Outlet中会显示带有index属性的路由

<Routes>
    <Route path='/foo' element={Foo}>
        <Route index element={Default}></Route>
        <Route path='bar' element={Bar}></Route>
    </Route>
</Routes>
    
    
4. 重定向路由  Navigate

import { Navigate, Route } from 'react-router-dom'

<Route to="/" element={ <Navigate to="/home" replace="true" /> } />
```



## 2. useRoutes的使用

```react
这个useRoutes很像vue中的静态路由的配置

import { useRoutes } from 'react-router-dom'

export default function Routes() {
    return useRoutes([
        {
            path: '/home',
            element: <Home />
        },
        {
            path: '/about',
            element: <About />
        }
    ])
}


配置路由最佳实践

router/index.js

import { Navigate } from 'react-router-dom'

const routes = [
    {
      path: '/',
      element: <Navigate to="/home" />
    },
    {
       path: '/home',
       element: <Home />
    },
    {
       path: '/about',
       element: <About />
    }
]

export default routes

APP.js

import { useRoutes } from 'react-router-dom'
import routes from ''

const router = useRoutes(routes)

<div>
      { router }
</div>
```



## 3. 嵌套路由

```react
import { Navigate, letout } from 'react-router-dom'

{
    path: '/home',
    element: <Home />
    children: [
        {
            path: 'email',
            element: <Email />
        },
        {
            path: 'tel',
            element: <Tel />
        },
        {
            path: '',
            element: <Navigate to="email" />
        }
    ]
}

路由的出口使用outlet
```

