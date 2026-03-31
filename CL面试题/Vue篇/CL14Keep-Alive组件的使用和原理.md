# keep-alive的使用和原理

## 1. keep-alive起到的作用

1. keep-alive组件是vue的内置组件，用于缓存内部组件实例，这样做的目的在于，**keep-alive内部组件切换时，不用重新创建组件实例，而直接使用缓存中的实例**，一方面能够避免创建组件带来的开销，另一方面可以保留组件的状态
2. keep-alive还有另外几个属性，include和exclude, 通过这些属性可以控制哪些组件可以进入缓存，还有一个max属性，设置最大的缓存数当缓存的实例超过该数时，vue会移除最久没有使用的组件缓存
3. 受到keep-alive的影响，其内部所有嵌套的组件都具有两个新的生命周期钩子函数，分别是activated和deactivated,他们分别在组建的激活和失活时触发，activated触发在mounted之后

```vue
<template>
  <button @click="click">
    切换组件  
  </button>
  <keep-alive>
      <Component :is="arr[index]"></Component>
  </keep-alive>
</template>

<script>
    export default{
        data(){
            return {
                arr: [Com1, Com2, Com3],
                index: 0
            }
        },
        methods: {
            click(){
                this.index = (index + 1) % 3 
            }
        }
    }
</script>

1. 有时候会有这样的场景，在一个页面填了大量的数据，不小心切换到了另一个页面，再切回来时希望数据还在，这个时候就可以利用keep-alive来保持活性
```



## 2. keep-alive的原理

1. 在具体的实现上，keep-alive在内部维护了一个keys数组和一个缓存对象

```vue
//kep-alive内部声明了周期函数

created(){
  this.cache = Object.create(null);    //缓存对象
  this.keys = []
}
```

2. keys数组记录目前缓存的组件的key值，如果组件没有指定key值，则会为其生成一个唯一的key值
3. cache对象以key值为键，vnode为值，用于缓存组件对应的虚拟Dom
4. 在keep-alive的渲染函数中，他会先判断当前渲染的vnode是否有对应的缓存，如果有从缓存中读取对应的组件的实例，如果没有将其缓存，当缓存的数量超过max数值时，keep-alive会移除掉key数组的第一个元素

```js
render(){
  const slot = this.$slots.default; // 获取默认插槽
  const vnode = getFirstComponentChild(slot); // 得到插槽中的第一个组件的vnode
  const name = getComponentName(vnode.componentOptions); //获取组件名字
  const { cache, keys } = this; // 获取当前的缓存对象和key数组
  const key = ...; // 获取组件的key值，若没有，会按照规则自动生成
  if (cache[key]) {
    // 有缓存
    // 重用组件实例
    vnode.componentInstance = cache[key].componentInstance
    remove(keys, key); // 删除key
    // 将key加入到数组末尾，这样是为了保证最近使用的组件在数组中靠后，反之靠前
    keys.push(key); 
  } else {
    // 无缓存，进行缓存
    cache[key] = vnode
    keys.push(key)
    if (this.max && keys.length > parseInt(this.max)) {
      // 超过最大缓存数量，移除第一个key对应的缓存
      pruneCacheEntry(cache, keys[0], keys, this._vnode)
    }
  }
  return vnode;
}
```

