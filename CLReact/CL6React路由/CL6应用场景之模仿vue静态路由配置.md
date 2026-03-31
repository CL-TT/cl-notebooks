# 应用场景之模仿vue静态路由配置

```js
1. 这个是路由配置文件

const routes = [
    {
        path: '/',
        exact: true,
        component: Home
    },
    {
        path: '/about',
        exact: true,
        component: About,
        children: [
            {
                path: '/news',
                exact: true,
                component: News
            },
            {
                path: '/music',
                exact: true,
                component: Music
            }
        ]
    }
]

export default routes;
```

```js
import { BrowserRouter as Router, Link } from 'react-router-dom'
import RootRouter from '';

function App(){
    return <Router>
      <div>
        <Link to="/">首页</Link>
        <Link to="/about">关于页面</Link>
      </div> 
      <div>
         <Rootrouter></RootRouter>
      </div>
    </Router>
}


function About(props){
    return <div>
        我是关于页面
        <div>
          <Link to="/about/news">新闻</Link>
          <Link to="/about/music">音乐</Link>
        </div>
        <div>
           { props.children }  =>  也是一个Route数组
        </div>
    </div>
}
```

## 封装一个能适配静态路由配置的组件

```js
import { Switch, Route } from 'react-router-dom'
import routes from ''

/**
* 获取route组件数组
* 1. 所以他的component属性不能直接给它， 需要借助render函数
* 2. render函数只有在完全匹配的时候才会运行，返回一个reactdom结构
**/
function getRoutes(routes, basePath){
    const rs = routes.map((item, index) => {
        const { children, component: Component, path, ...res } = item
        let newPath = `${ basePath }${ path }`
        newPath = newPath.replace(/\/\//g, '/')
        return <Route key={index} { ...res } render={ (val) => {
            return <Component { ...val } >{ getRoutes(children, newPath) }</Component>
        } } ></Route>
    })
}

function RootRouter(){
    
    return <Switch>
        { getRoutes(routes, "/") }
    </Switch>
}

export default RootRouter
```

