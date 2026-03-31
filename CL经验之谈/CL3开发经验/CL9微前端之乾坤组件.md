# 微前端之乾坤组件

## 1. 什么是微前端

​     微前端借鉴了微服务的架构理念，**将一个庞大的前端应用拆分为多个独立灵活的小型应用**，每一个应用都可以独立开发，独立运行，独立部署， 再将这些小型应用联合为一个完整的应用，**微前端既可以将多个项目融合为一，又可以减少项目之间的耦合，提升项目扩展性**，相比一整块的前端仓库，微前端架构下的前端仓库倾向于更小更灵活



## 2. 乾坤(qiankun)的使用

### 2.1 主应用（localhost:8080）

主应用不限技术栈，只需要提供一个容器Dom，然后注册微应用并start即可

```js
1. 先安装  npm install qiankun

2. 在src下创建一个子应用配置文件，暂且叫做micro_app.js

const microApps = [
    {
        name: 'micro_vue',
        entry: '//localhost:8081/',
        activeRule: '/micro_vue',
        container: '#subapp-viewport', // 子应用挂载的div
        props: {
            routerBase: '/micro_vue' // 下发路由给子应用，子应用根据该值去定义qiankun环境下的路由
        }
    }
]

export default microApps

3. 改写app.vue文件

加一个能提供子应用匹配的容器
<div id="subapp-viewport"></div>   这个id和配置项中的container值相同


4. 改写main.js文件， 将配置的子应用进行注册并启动qiankun框架服务

import { registerMicroApps, start } from 'qiankun';
import appConfig from './micro_app';

// 注册子应用
registerMicroApps(appConfig, {
   beforeLoad: app => {
     console.log('before load app.name====>>>>>', app.name)
   },
   beforeMount: [
     app => {
        console.log('[LifeCycle] before mount %c%s', 'color: green;', app.name);
     },
   ],
   afterMount: [
     app => {
        console.log('[LifeCycle] after mount %c%s', 'color: green;', app.name);
     }
   ],
   afterUnmount: [
     app => {
       console.log('[LifeCycle] after unmount %c%s', 'color: green;', app.name);
     },
   ],
});
   
// 启动qiankun
start();
```





### 2.2 子应用(暂时只针对vue项目) (localhost:8081)

```js
1. 在项目的根路径创建vue.config.js进行配置

const { name } = require('./package.json')

module.exports = {
  configureWebpack: {
    output: {
      library: `${name}-[name]`,
      libraryTarget: 'umd',
      jsonpFunction: `webpackJsonp_${name}`,
    }
  },
  devServer: {
    port: 8081, // 在.env中VUE_APP_PORT=7788，与父应用的配置一致
    headers: {
      'Access-Control-Allow-Origin': '*' // 主应用获取子应用时跨域响应头
    }
  }
}


2. 在src目录下面创建public-path.js文件， 这个文件用来改写webpack打包之后通过哪个路径可以访问到打包后的文件

(function() {
  // 用来判断是否运行在乾坤的框架下
  if (window.__POWERED_BY_QIANKUN__) {
    // eslint-disable-next-line no-undef
    __webpack_public_path__ = window.__INJECTED_PUBLIC_PATH_BY_QIANKUN__;
  }
})();


3. 修改router下的index.js文件，只需要导出对应的路径定义，不需要导出vue-router实例

export default routes


4. 改写main.js文件

import './public-path'  
import Vue from 'vue'
import App from './App.vue'
import routes from './router'
import store from './store'
import VueRouter from 'vue-router'
Vue.use(VueRouter)
Vue.config.productionTip = false

let instance = null
window.projectName = 'micro_vue';

function render (props = {}) {
  // 这个是我们在父类注册的时候定义的那些参数。
  const { container, routerBase } = props
  const router = new VueRouter({
    base: window.__POWERED_BY_QIANKUN__ ? routerBase : process.env.BASE_URL,
    mode: 'history',
    routes
  })
  
  instance = new Vue({
    router,
    store,
    render: (h) => h(App)
  }).$mount(container ? container.querySelector('#app') : '#app')
}
      
if (!window.__POWERED_BY_QIANKUN__) {
   render()
}
      
export async function bootstrap () {
   console.log('[vue] vue app bootstraped')
}
      
export async function mount (props) {
   console.log('[vue] props from main framework', props)
   render(props)
}

export async function unmount () {
   instance.$destroy()
   instance.$el.innerHTML = ''
   instance = null
}


5. 最后在主应用中访问  localhost:8080/micro_vue
```

