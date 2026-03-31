# UmiJS

## 1. umi简介

1. 全局安装 yarn global add umi
2. 运行 umi dev
3. 打包 umi build



## 2. umi中的路由

### 1. 约定式路由

1. umi约定， **工程中pages文件夹存放的是页面，** 如果工程包含src目录，则src/pages是页面文件夹
2. umi约定，页面的文件名，以及页面的文件路径，是该页面匹配的路由
3. umi约定，如果页面的文件名是index, 则可以省略文件名（首页）
4. umi约定， 如果src/layouts目录存在的话，**则该目录的index.js表示的是全局的通用布局， 布局中的children则会添加可变的页面**
5. umi约定， 如果pages文件夹中包含_layout.js, 则__layout.js所在的目录以及其所有的子目录中的页面，共用该布局
6. 404约定，pages/404.js, 表示404页面， 如果路由无匹配，则会渲染该页面， 该约定在开发模式无效，只有在部署后生效
7. 如果文件名为[名称].js， 则为动态路由



### 2. 路由跳转

```js
import { Link, NavLink, history } from 'umi';

// 导航式跳转

<Link to="" />
<NavLink to="" />
    
    
// 编程式跳转
histoty.push('/about');
    
```



### 3. 配置式路由

1. **当使用了配置式路由，约定式路由全部失效**
2. 两种方式书写umi配置
3. 使用根目录下的文件  .umirc.js
4. 使用目录下的文件 config/config.js
5. 进行路由配置时，每一个配置就是一个匹配规则，并且每个配置是一个对象，对象中的某些属性，会直接形成Route组件的属性

```js
// 1. component配置项： 需要填写页面组件的路径，路径相对于pages文件夹
// 2. 如果配置项没有exact, 则会自动添加exact为true
// 3. Routes配置，是一个数组，数组的每一项是组建的路径，这个路径是相对于项目根路径的， 起到了导航守卫的作用， 有了这个配置他不会渲染component属性， 而是会把component作为他的children属性， 数组如果有多个组件的话，后面的总是前面的children属性 

export default {
    routes: [
        {
          path: '/',
          component: '../layouts/index',
          //嵌套路由
          routes: [
              {
                  path: '/',
                  component: './index',
                  title: '首页',
                  Routes: ['组件的路径']
               },
               {
                  path: '/login',
                  component: './login',
                  title: '登录'
               }
          ]
        }
    ]
}
```

```js
// 1. 路由配置中的信息，同样可以放到约定式路由中，方式是，为约定式路由添加一个文档注释(注释的格式是YAML格式), 这是2.0版本的做法

// 2. 注释需要放到头部


/**
* title: 首页
* Routes: 
*     - ./src/routes/......
*     - ./src/routes/......
**/



// 3.0版本的做法

function Index(){
    return (
    )
}

Index.title = ''

Index.wrappers = '@/'
```







## 3. umi中使用dva

1. 需要下载 yarn add @umijs/preset-react
2. 在配置文件中进行配置.umirc.js

```js
export default {
    dva: {
        
    }
}
```

3. dva运行时的配置

```js
在src/app.js文件里

import { createLogger } from 'redux-logger';

export const dva = {
    config: {
        onAction: createLogger()
    }
}
```





## 4. 样式

1. 全局样式：umi中约定src/global.css为全局样式，如果存在此文件，会被自动引入到入口文件最前面
2. 局部样式：src/assets/css文件夹里面
3. 内置支持css，less， 不支持sass



## 5. 代理

```js
在配置文件中

1. 以api开头的接口，会以target的地址去请求， changeOrigin表示源地址也会换成target的地址

2. pathRewrite的作用就是, 如代理请求的地址是  http://clei.top/api/findAll,   但实际的地址是http://clei.top/findAll,  所以就需要把api给去掉

export default{
    proxy: {
        '/api': {
            target: '',
            changeOrigin: true,
            pathRewrite: : {
              "^/api": '/'
            }
        }
    }
}
```



## 6. mock数据

1. 约定在根目录的mock文件夹下书写js文件

```js
export default {
    'GET /api/findAll': {
        message: '请求成功',
        status: 200,
        data: []
    }
}
```



## 7. umi脚手架

1. yarn create umi ， 创建umi工程