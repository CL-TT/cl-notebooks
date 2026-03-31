# 路由篇VueRouter

[TOC]

# 1.路由的基本使用

```vue
//1. 引入模块，路由是基于vue模块的

import Vue from 'Vue';

import VueRouter from 'vue-router';

//2. Vue使用VueRouter

Vue.use(VueRouter);


//3. 创建路由数组 => 相当于路由表

const routes = [
  {
    //路由的路径
    path: '',

    //加载的组件
    component: () => import (''),    //路由懒加载

    //命名路由
    name: '',

    //重定向
    redirect: '',

    //别名
    alias: '',

    //嵌套路由
    children: [
       {
         path: ''
       }
    ]
  }
];

const router = new VueRouter({
  routes,
  mode: 'history'
});

export default router;


//4. 声明式导航

<router-link to=""></router-link>

<router-view>进行内容展示</router-view>
```



## 2.路由的理论名词

1. 命名路由    name    在使用的时候 :to="{name: name值}"

2. 嵌套路由    子路由

3. 重定向  redirect

4. 别名  alias

5. 重定向和别名的区别

   路径A，  重定向成路径B，  会把 /A 变成 /B   路径匹配的是 /B

   路径A， 起别名成路径B       /A  =>  /B     访问 /A 匹配的路径还是 /A       访问别名 /B  匹配的路径还是 /A



## 3.编程式导航

```
1. $router的基本使用

1.1 this.$router.push方法

比如说你想跳转
this.$router.push('/home') 
this.$router.push({})

方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时， 则回到之前的 URL
例如： [a, b, c]   push(d)  =>  [a, b, c, d]  回退会回到c

1.2 this.$router.replace('/home') 

同样可以跳转到home页面，与push不同的是在回退时要注意  
[a, b, c]  replace(d) => [a, b, d]   回退会回到b


1.3 this.$router.go(数字)  前进多少或者后退多少


2. $route的使用

3. $route的使用{
       *        1. $route是一个只读，路由信息对象。
       
       *        2. $route.path
       *           字符串，对应当前路由的路径，总是解析为绝对路径，如 "/foo/bar"。
       
       *        3. $route.params
       *           一个 key/value 对象，包含了动态片段和全匹配片段，如果没有路由参数，
       *           就是一个空对象。
       
       *        4. $route.query
       *           一个 key/value 对象，表示 URL 查询参数。例如，对于路径 /foo?user=1，
       *           则有 \$route.query.user == 1，如果没有查询参数，则是个空对象。
       
       *        5. $route.hash
       *           路由的 hash 值 (带 #) ，如果没有 hash 值，则为空字符串。
       
       *        6. $route.fullPath
       *           完成解析后的 URL，包含查询参数和 hash 的完整路径。
       
       *        7. $route.matched
       *           一个数组，包含当前路由的所有嵌套路径片段的路由记录 。
       *           路由记录就是 routes 配置数组中的对象副本 (还有在 children 数组)
       
       *        8. $route.name
       *           当前路由的名称，如果有的话
       
       *        9 .$route.redirectedFrom
       *           如果存在重定向，即为重定向来源的路由的名字。
       *      
  }
```





## 4.动态路由匹配

```vue
1. 动态路由的运用场景

打个比方，比如在写商品详情的时候，页面结构都一样，只是商品的id不同，所以这个时候就可以用动态路由

2. 基本使用

/question/12387
/question/17867
/question/34987

//路由配置
{
   path: '/question/:id',    //只要是动态的数据都写成:id
   component: () => import (''),
   name: 'question'
}

//路由跳转
<router-link :to="{name: 'question', params: { id: id }}"></router-link>
跳转过后的参数可以使用$route路由信息对象来获取

const params = this.$route.params;

//同样可以在watch中监听路由信息的变化
'$route': {
   handler: () => {

   }
}

```





## 5.命名视图以及路由传参

```vue
1. 命名视图

<router-view name="名称"></router-view>

<router-view></router-view>
如果没有名称的话，默认就是default

{
   components: {
      名称：() => import '',

      default: () => import ''
   }
}


2. 路由传参

1. 在组件中使用$route会使其与路由形成高度耦合， 从而使组件只能在某些特定的url上使用，限制了其灵活性， 在路由表中使用props将组件和路由解耦

2. 如果props设置为true，只会将route.params作为组件的属性

3. 如果props是一个对象，他会被按原样设置为组件属性

4. const router = new VueRouter({
   routes: [
      {
         path: '',
         props: true || {}
         
         //props还可以是一个函数
         props: route => ({
           id: route.params.id,
           name: route.name
         })
      }
   ]
})
```

