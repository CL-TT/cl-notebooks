# React路由

## 1. 最佳实践 v6.4以前

```jsx
1. 安装路由 

npm i react-router-dom
yarn add react-router-dom
yarn add react-router

2. 配置路由的使用

第一步在main.jsx中

import { BrowserRouter } from 'react-router-dom'
createRoot(document.getElementById('root')).render(
  <StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </StrictMode>,
)

第二步在router.jsx中

import { Navigate } from 'react-router-dom'
import Home from '@/views/home'
import About from '@/views/about'
import { lazy, Suspense } from 'react'

// 懒加载路由
const Login = React.lazy(() => import('@/pages/Login'));
// 加载占位组件（可选，替代 Vue 的 loading 组件）
const Loading = () => <div>加载中...</div>;

const routes = [
  {
    path: '/',
    element: <Navigate to="/home" />
  },
  {
    path: '/home',
    element: <Home />
    // 嵌套路由
    children: [
      
    ]
  },
  {
    path: '/about',
    element: <About />
  },
  {
    path: '/login',
    // 用 Suspense 包裹懒加载组件，指定加载占位
    element: (
      <Suspense fallback={<Loading />}>
        <Home />
      </Suspense>
    )
  }
]

export default routes


第三步在app.jsx中去使用

import { NavLink, useRoutes } from 'react-router-dom'
import routes from '@/router/router'

function App() {
    const router = useRoutes(routes)
    
    return (
      <div>
        <nav></nav>
        <div>
          { router }    
        </div>
      </div>
    )
}

如果有嵌套路由， 路由的出口使用<outlet />
```



## 2. 最佳实践 v6.4 之后  RouterProvider

```

```



## 2. 常用的组件和Hook

```jsx
1. 路由相关的组件

1.1 <BrowserRouter />   history模式的路由
1.2 <HashRouter />      哈希路由
1.3 <Routes />
1.4 <Route />
1.5 <Link />  跳转组件
1.6 <NavLink to="" />  跳转组件，高亮
1.7 <Navigator to="" />   


2. 相关的hook

2.1 useNavigator   命令行跳转
2.2 useLocation    获取路径跳转时的附带的参数
2.3 useParams      获取路径上的参数
```

