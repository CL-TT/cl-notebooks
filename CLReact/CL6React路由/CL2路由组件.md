# 路由组件

## 1. Router组件

1. 它本身不作任何的展示， 仅提供路由模式配置， 另外该组件会产生一个上下文，上下文会提供一些实用的对象和方法供其他相关组件使用
2. 两种模式

```html
import { HashRouter, BrowserRouter } from 'react-router-dom'

<script>
    function App(){
        return (
            <HashRouter />
        )
    }
</script>
```





## 2. Route组件

1. 根据不同的地址展示不同的组件

```js
1. path: 匹配的路径

默认情况下，不区分大小写，可以设置sensitive属性为true。来区分大小写

默认情况下，只匹配初始目录，如果要精确匹配的话，配置exact属性为true

如果不写path，则会匹配任意路径


2. component: 匹配成功之后要显示的组件  =>  被改成element属性

3. children

传递react元素，无论是否匹配，一定会显示children，并且会忽略component属性

传递一个函数，该函数有多个参数，这些参数来自于上下文，该函数返回react元素， 则一定会显示返回的元素， 并且忽略component属性

4. render函数

<Route render={ () => {} } />

render是匹配过后才会运行， children是无论是否匹配都会运行
```





## 3. Switch组件  （v6 => 已经被改成Routes组件）

1. 写到switch组件中的route组件， 当匹配到第一个route后，会立即停止匹配
2. 由于switch组件会循环所有子元素， 然后让每一个子元素完成匹配， 若匹配到，则渲染对应的组件，然后停止循环， 因此，不能在switch的子元素中使用除route外的其他组件





```js
// 新版是这样写了

import { HashRouter, Routes, Route } from 'react-router-dom';

function A(){
  return (
      <div>我是a组件</div>
  )
}

function B(){
    return (
        <div>我是b组件</div>
    )
}

function App(){
    return (
        <HashRouter>
          <Routes>
            <Route path="/a" element={ <A /> } />
            <Route path="/b" element={ <B /> } />
          </Routes>
        </HashRouter>
    )
}
```





## 4. v6版本之后 react-router-dom的使用

```js
1. 安装 npm i react-router-dom

2. ***********基本的路由实现************

router一定是放在最顶层的

import { BrowserRouter as Router, Routes, Route } from 'react-router-dom'

function App(){
    return (
        <Router>
          <Routes>
            <Route path="" element={}></Route>
            <Route path="" element={}>
               <Route path="" element={}></Route>
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

嵌套路由需要在出处写上 <Outlet />组件
```

