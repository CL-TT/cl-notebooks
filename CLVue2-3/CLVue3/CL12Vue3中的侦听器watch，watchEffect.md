# 侦听器watch, watchEffect

## 1. watch的使用

```vue
<script>
    import { watch } from 'vue'
    
    watch(prop, () => {
        // 当prop发生变化的时候，就会触发这个回调函数
    })
</script>


1. 数据源有哪些

watch(name, () => {})
watch(对象， () => { 对象里面的属性发生变化 })
watch(计算属性, () => {})
watch(getter函数， () => { 当函数的返回值发生变化时才会重新执行 })
watch([name, age], () => { 监听多个数据组成的数组 })
watch(()=>obj.name, () => { 监听对象中的某个属性 })


2. 监听reactive，对象时是深层次的，但是对于复杂的数据结构，是非常消耗性能的

3. 第三个参数

#1. immediate: true  立即执行一次
#2. once: true   只执行一次
#3. deep: true  是否开启深层次的监听
#4. flush: post  在dom更新之后再回调这个函数

4. 回调函数的执行时机

在父组件的dom渲染完成之后，在本组件的dom渲染之前调用， 所以是获取不到更新过后的dom值的
```



## 2. watchEffect的使用

```vue
<script>
  import { watchEffect } from 'vue'
    
  watchEffect(() => {
      // 
  })  
</script>
```

特点

1. watchEffect不需要显示的指定响应式数据依赖，在回调函数中用到了哪一个响应式数据，该数据就会成为一个依赖
2. watchEffect会立即执行一次