# 为什么Vue3去掉了Vue的构造函数

## 1. Vue2的场景

```js
import Vue from 'vue';

Vue.use();    //使用插件
Vue.mixin();  //混合
Vue.component();  //使用组件

new Vue({})

1. 如果遇到一个页面多个Vue应用， 这样写会影响到所有的Vue应用， 往往就会遇到一些问题
```



## 2. Vue3的场景

```js
import { createApp } from 'vue';

createApp(App).mount('#app');

1. 如果存在多个Vue应用的话，新版的是这样处理的
createApp().use().mixin().mount()
这样就不会影响其他的Vue应用
```



## 3. 标准回答

Vue2的全局函数带来了许多的问题

1. 调用构造函数的静态方法会对所有的Vue应用生效，不利于隔离不同的应用
2. Vue2的构造函数集成了太多的功能，而Vue3把这些功能使用普通函数导出，能够充分优化打包体积
3. Vue2没有把组件实例和Vue应用两个概念区分开来，在Vue2中，通过new Vue创建的对象，既是一个Vue应用，同时又是一个特殊的Vue组件。 而在Vue3中，把两个概念区分开了，通过createApp创建的对象是一个Vue应用，它内部提供的方法是针对整个应用的，而不再是一个特殊的组件