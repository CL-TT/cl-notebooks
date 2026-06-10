# Vue3中的组件通信之v-model

v-model是vue的一个内置指令，它不仅可以作为数据的双向绑定来使用，同样可以作为组件之间通信的桥梁



defineModel本质上就是prop + emit

```vue
父组件

<template>
  <div>
    <Comp v-model="count" />
  </div>
</template>

<script>
import { ref } from 'vue'
    
const count = ref(0)
</script>



子组件

<template>
  <div>
    {{ count }}    
      
    <button @click="change">
        改变count的值
    </button>
  </div>
</template>

<script>
import { defineModel } from 'vue'
    
const count = defineModel()  // 这个count就是一个ref值

const change = () => {
    count.value = 10
}
</script>



特点：
1. defineModel中可以做一些校验

defineModel({
  type: '',
  required: true,
  default: 0
})


2. v-model:title   当有多个model的时候，来表示这个model的具体名称


3. v-model.修饰符

这个修饰符在子组件里面起作用，用来限制对父组件数据的修改

const [count, obj] = defineModel({
  set(val) {
     if(obj.修饰符名称) {

     }

     return val
  }
})
```

