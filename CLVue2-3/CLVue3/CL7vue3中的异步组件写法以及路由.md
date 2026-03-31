# vue3中的异步组件以及路由

## 1. 异步组件

1. defineAsyncComponent
2. 封装异步组件，和异步页面的方法

```js
import { defineAsyncComponent } from 'vue';

/**
* 导入异步组件
* path: 组件路径
**/
export function asyncComponent(path){
    return defineAsyncComponent({
        loader: () => {
            return import(path)
        },
        
        //加载中，要渲染的组件
        loadingComponent: Loading,
        
        //加载出错，要渲染的组件
        errorComponent: ErrorCom,
    })
}


export function asyncPage(path){
    return defineAsyncComponent({
        loader: () => {
           return import(path)
        }
    })
}
```



## 2. vue3中的路由

```js
npm install vue-router@next

import { createRouter, createWebHistory } from 'vue-router';

const routes = [
    {
        path: '',
        title: '',
        component: () => import('')
    }
]

export default createRouter({
    histort: createWebHistory(),
    routes
})


1. 这个history配置就是当初的mode配置， 来选则路由的路径形式
```

