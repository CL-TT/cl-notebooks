# Vue3的重大变化

## 1. vite构建工具

```javascript
1. 搭建工程 npm init vite-app vue3-vite，  init vite-app临时构建，构建完成之后立马移除vite


2. 创建vue项目

npm create vue@latest

yarn create vue@latest

pnpm create vue@latest


3. 创建vue3 + vite项目

npm init vite@latest

npm create vite@latest
```



## 2. Vue3的重大变化

### 1. vue实例的创建的变化

```vue
1. vue2实例的创建

import Vue from 'vue';

new Vue({
  render: h => h(app)
}).mount('#app')


2. vue3实例的创建

import { createApp } from 'vue';

createApp(app).mount('#app');

3. 在打印这个this实例，他是一个代理对象，如下图

```

![vue3-proxy](D:\CL武林秘籍\画图\vue3-proxy.jpg)

### 2. setup

1. 以前我们写vue代码都是用options API这种形式，一个完整的功能代码会被拆分到各个配置中， 如果这个组件的功能一多，极其难以阅读和维护

2. vue3使用composition Api
3. 这种写法最厉害的就是可以把一个功能模块抽离出去

```vue
1. setup的运行时间是在所有生命周期函数之前
2. 在setup函数里打印this，是undefined
3. 在setup中定义的变量函数，经过return返回，会被映射到实例中，所以在模板中可以使用
4. 在模板中又可以直接用countRef访问，如下图所示
5. 在setup函数中，必须使用countRef.value的形式访问数据

<template>
    <div>
      {{ count }}  => 1
    </div>
</template>

<script>
    import { ref } from 'vue'; 
    
    export default{
        setup(){
            console.log('setup的运行时间是在所有生命周期函数之前')
            
            const count = 1;   //这种定义方式数据是不具有响应式的，所以需要composition API
            
            const countRef = ref(1);   //这样的形式，数据是具有响应式的，count是一个对象，值在value属性中
            
            return {
                count
            }
        }
    }
</script>
```

![vue3-ref](D:\CL武林秘籍\画图\vue3-ref.jpg)

### 3. 自定义组件上的双向数据绑定的改变

```
1. 在vue2中自定义组件的双向数据绑定有两种方式 v-model和sync修饰符

   v-model的实质  :value  @input

   sync的实质是  :title.sync   =>    :title    @update:title
   

<Com :value="value" @input="handleInput" />    =>   <Com v-model="value" />

<Com :title="title" @update:title="handleTitle" />   =>   <Com :title.sync="title" />


2. 在vue3中去掉sync修饰符， 并且v-model可以绑定多个数据

  v-model的实质是   :modelValue   @update:modelValue
  当绑定多个值的时候，v-model:title=""
  
  <Com v-model="" v-model:title="" />
  

3. 是可以加修饰符的，只不过修饰功能需要自己去实现

<Com v-model.trim="title" />

在子组件中
props: {
  modelModifiers: {
    default: () => ({})
  }
}

modelModifiers就是一个对象，里面有trim这个属性

```





## 4. v-if和v-for

1. v-if 的优先级现在要高于 v-for
