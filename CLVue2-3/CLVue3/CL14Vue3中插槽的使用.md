# Vue3中插槽的使用

当需要向子组件传递模板内容时，使用props是做不到的，那么就需要插槽来实现

具名插槽  v-slot:body   ==   #body

作用域插槽：子组件的数据可以提供给父组件使用

## 1.基本使用

```vue
<template>
  <div>
    <Comp >
      <div>默认插槽的内容</div>
        
      <template #body>具名插槽的内容</template>

      <template v-slot:content="{ count }"></template>   // 通过结构来获取数据
    </Comp>    
  </div>
</template>



子组件

<template>
  <div>
    <slot></slot>  // 默认插槽
      
    <slot name="body"></slot>   // 具名插槽
      
      
    <slot name="content" :count="count"></slot>   // 作用域插槽
  </div>
</template>
```

