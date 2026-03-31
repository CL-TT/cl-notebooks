# VueDraggable

VueDraggable是一款基于Sortable.js实现的

```vue
1. 安装

npm i vuedraggable

2. 使用

v-model会绑定一个数组

group: 分组名称，相同的分组之间可以相互拖拽

start: 拖拽开始时能做什么

end: 拖拽结束之后能干什么

<vuedraggable v-model="arr" group="" @start="" @end="" itemKey="">
    element可以起别名
    <template #item="{ element:item, index }">
      <div>{{ element.name }}</div>
    </template>
</vuedraggable>
```

