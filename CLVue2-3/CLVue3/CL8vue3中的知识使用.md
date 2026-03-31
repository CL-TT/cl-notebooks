# vue3中的知识使用

## 1. 定义响应式数据

```js
import { ref, reactive, readonly } from 'vue';

const nameRef = ref('caolei');

const obj = reactive({
    name: 'caolei',
    age: 20
})

const readOnly = readonly({
    name: 'lyt'
})
```



## 2. 计算属性

```js
import { computed, ref } from 'vue'

const firstNameRef = ref('cao');
const secondNameRef = ref('lei');

const fullName = computed(() => {
    return firstNameRef.value + secondName.value;
})
```



## 3. 监听器

```js
import { watch } from 'vue';

watch(firstNameRef, () => {
    console.log('我在监听响应式数据，firstName的变化');
})
```



## 4. vuex的使用

```js
如果想要在js文件中单独使用store的话

import { useStore } from 'vuex';

const store = useStore();  

store.state

store.dispatch('', payload);
```



## 5. 路由的使用

```js
import { useRouter, useRoute } from 'vue-router';

const router = useRouter();

const route = useRoute();
```



## 6. 父子组件之间的数据交互

```js
//子组件

import { defineProps, defineEmits } from 'vue';

//先定义好emits的事件
const emit = defineEmits(['changeAge']);

//定义emit事件和它的参数
const emit = defineEmits<{
    (e: 'update', id: number): void
    (e: 'change', value: string): void
}>()

// 相当于vue2中的props写法
const props = defineProps({
    age: {
        type: Number,   //类型校验
        default: 20,    //默认值
        required: true, //是否必传
    }
})

const changeAge = (payload): void => {
    emit('changeAge', payload);
}
```



## 7. 全局变量的使用

```js
1. 在main.js

app.config.globalProperties.$bus = ...


2. 在vue文件中使用

import { getCurrentInstance } from 'vue'

const { proxy } = getCurrentInstance()

proxy.$bus......
```



## 8. 生命周期函数

```vue
import { onMounted, onUpdate } from 'vue'

最常用的两个
```



## 9. 绑定元素ref的使用

```vue
import { ref } from 'vue'

<div ref="divRef"></div>

const divRef = ref(null)    // 变量名和元素上绑定的ref名称一致

divRef.value  // 就能拿到div元素

在onMounted里面才能获取到Dom
```

