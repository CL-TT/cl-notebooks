# 动态路由的创建流程

## 1. 什么是动态路由

​        动态路由，顾名思义，是可以动态变化的路由， 可以根据不同的需求加载不同的路由，做到不同的实现以及页面渲染。动态路由的存储可以分为两种，一种是将路由存储到前端，另一种则是将路由存储到数据库，动态路由一般结合角色权限控制使用



## 2. 动态路由的实现（路由存储在数据库）

```js
1. 在登录之后拿到后端返回的路由数据，并且把它存储到vuex中

2. 然后在路由前置守卫中通过route.addRoute来添加动态路由

let idAdd = false

router.beforeEach((to, from, next) => {
    const token = localStorage.getItem('token')
    
    if (token) {
        // 登录过了
        if (to.path === '/login') {
            next({ name: 'Home' })
        } else {
            if (!isAdd && asyncRoutes.length) {
                // 添加动态路由
                asyncRoutes.map(item => {
                    router.addRoute(item)
                })
                
                isAdd = true
                
                next({ ...to, replace: true })
            } else {
                next()
            }
        }
    } else {
        
    }
})

3. 注意通过addRoute添加的路由，用router.options.routes是访问不到的
```



## 3. vuex持久化数据的方法

```js
1. 第一种使用localStorage和sessionStorage

2. 第二种使用vuex-persistedstate库  (原理同样也是借助localStorage)

npm i vuex-persistedstate

import createPersistedState from 'vuex-persistedstate'

export default new Vuex.store({
    plugins: [ createPersistedState() ],
    state: {}
})
```

