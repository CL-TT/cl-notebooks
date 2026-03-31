# vue3状态管理工具pinia的使用

## 1. 创建一个store

```js
import { defineStore } from 'pinia'

export const userStore = defineStore('user', {
    // 相当于data的作用
    state: () => {
        return {
            name: 'caolei',
            age: 20,
            sex: '男'
        }
    },
    // 相当于computed的作用
    getters: {
        ageComputed() {
            return this.age + 20
        }
    },
    // 逻辑处理，相当于methods的作用,  需要改变仓库中的数据，需要借助actions
    actions: {
        setName() {
            this.name = ''
        }
    }
})


两种方式

import { defineStore } from 'pinia'

import { ref, computed } from 'vue'

export const useCounterStore = defineStore('counter', () => {
    const count = ref(0)   // 相当于state
    
    const num = computed(() => {})   // 相当于getter
    
    // 相当于action
    function add() {
        
    }
    
    return {
        count,
        num,
        add
    }
})
```



## 2. 使用

```js
import { userStore } from '@/store'
import { storeToRefs } from 'pinia'

const store = userStore()

// 在页面中使用时，如果想要store中的数据变成响应式的
const { name, age, sex } = storeToRefs(store)

//需要改变仓库中的值需要借助actions来实现
store.setName()

// 改变store中的值
store.name = 'kkk'

// 批量更改
store.$patch({
    name: 'kk',
    age: 30
})  


// 重置store中的值
store.$reset()
```



## 3. pinia（轻量化）和vuex的区别

```js
1. pinia是一个轻量级的状态管理库，而vuex是一个更完整的状态管理库，它提供了更多的功能，比如模块化，插件，和严格模式

2. pinia可以创建多个store实例， 但是vuex只能创建一个
```



## 4. pinia的持久化方案

```js
1. 安装插件  npm i pinia-plugin-persistedstate

2. 将插件添加到实例上

import { createPinia } from 'pinia'

import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'

const pinia = createPinia()

pinia.use(piniaPluginPersistedstate)

3. 使用

import { defineStore } from 'pinia'

const useUserStore = defineStore('user', ({
    state: () => {
        
    },
    
    actions: {
        
    },
    
    persist: true // 这个要设置为true
    
    persist: {
      // 或者进行自定义配置
    }
}))
```

