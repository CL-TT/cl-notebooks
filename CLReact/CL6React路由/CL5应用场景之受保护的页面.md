# 应用场景之受保护的页面

## 1. react-router-dom  v6版本之前

```js
import { BrowserRouter as Router, Route, Switch, Redirect } from 'react-router-dom'

function App(){
    return (
        <Router>
          <Switch>
            <Route path="/login" component={} />
            <Route path="/protect" component={} />
            <Route path="/" component={} />
                
          </Switch>
        </Router>
    )
}


//自己定义一个受保护的路由组件
function ProtectRouter({ component: Component, children, render, ...res }){
    return (
        <Route { ...res }  render={ () => {
        //判断是否登录来决定渲染什么
        if(isLogin){
            //如果已经登录了，直接跳转到受保护的页面， 
            return <Component />
        }else{
            //没有登录就跳转到登录页
            return <Redirect to={ { pathname: '', search: '' } } />
        }
    } } />
    )
}

```





## 2. v6版本这之后

1. v6版本之后就报错， 不知道为什么？