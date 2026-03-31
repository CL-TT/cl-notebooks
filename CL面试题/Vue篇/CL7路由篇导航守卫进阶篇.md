# 进阶篇之导航守卫

## 1.导航守卫的理解

1. 导航：路由正在发生变化
2. 导航守卫：主要通过跳转或者取消的方式守卫导航
3. 导航守卫主要被分为三种：全局性的，单个路由独享的，组件内的



## 2.全局守卫

```vue
const router = new VueRouter({
  routes: [
    
  ]
});

1. 全局前置守卫 钩子函数beforeEach  在路由没有跳转之前就发生了

router.beforeEach((to, from, next) => {
   to: 去哪里
   from: 来自哪里
   
   next: 相当于在这个路程中的通行证
     1. next()  必须执行next函数才可以通往下一个路由
     2. next(false)  可以终止通往下一个路由
     3. next(路由地址) 可以通往其他的路由， 写法跟$router.push()写法一样
     4. next(error)  2.4+  如果传入的参数是一个error实例，导航则会被中止，并且该错     误会被传递给router.onError()注册过的回调函数
})


2. 全局解析守卫 钩子函数beforeResolve

在路由跳转之前触发，在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用，  会比beforeEach稍晚一点触发

router.beforeResolve((to, from, next) => {})


3. 全局后置守卫 钩子函数afterEach

在路由跳转完成之后触发
router.afterEach(() => {})
```



## 3.单个路由独享的守卫  beforeEnter

```vue
意思就是：指定的那个路由设置守卫，只要去了那个指定的路由，都要经过守卫的询问，但其他的路不归我管

const routes = [
  {
    path: '',
    component: () => import '',
    beforeEnter: (to, from, next) => {
       console.log('beforeEnter');

       next();
    }
  }
]

执行时间在beforeEach之后，  但是在beforeResolve之前
```



## 4.组件内守卫

组件内守卫是指在组件内执行的钩子函数，类似于组件的生命周期函数， 只要为这个组件添加了守卫， 那么使用过这个组件的路由都会被检验

```vue
export default{
  data(){
    return {}
  },

  //1. beforeRouteEnter 在路由进入之前调用
  beforeRouteEnter(to, from, next){
    1. 在该函数内访问不到该组件的实例，因为在路由进入之前调用，该实例还没有被创建，          this是指向undefined
    2. 可以在这里进行网络请求 axios,  把得到的结果可以传给next函数,  成功获取并且能        够进入到路由时，调用next并在回调通过 vm访问组件实例进行赋值等操作，next中的函        数调用在mounted之后，这是为了确保对组件实例的完整访问

    next(vm => {
       vm.setDatas(res.data);
    })
  },

  //2. beforeRouteUpdate 在当前路由改变，并且该组件被复用时调用
  beforeRouteUpdate(to, from, next){
     1. 可以通过this来访问实例
     2. 组件何时被复用  @1动态路由间相互跳转 @2路由query变更 
     next();
  }

  //3. beforeRouteLeave  导航离开该组件的对应路由之前调用
  beforeRouteLeave(to, from, next){
    1. 可以通过this访问实例
     
    next();
  }
}
```



## 5导航的完整解析流程

从路由A  =>    跳转到路由B



1. 导航被触发
2. 在失活的组件中，触发beforeRouteLeave

