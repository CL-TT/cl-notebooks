# Vue3的效率提升主要表现在哪几个方面

## 1. 静态提升

1. 静态元素会被提升
2. 静态属性也会被提升

```js
1. 在vue2中

render(){
    createVNode('h1', null, 'Hello World')
    // ...
}

2. 在vue3中, 静态节点，一些不变的元素都提出来

const h = createVNode('h1', null, 'Hello World')

render(){
    //直接使用h即可
}
```

```html
1. 静态属性会被提升

<div class="user">
    {{ user.name }}
</div>

const h = { class: 'user' }

render(){
  createVNode('div', h, user.name)
}
```



## 2. 预字符串化

1. 当编译器遇到大量连续的静态内容， 会直接将其编译为一个普通的 字符串节点



## 3. 缓存事件处理函数

```html
<button @click="add">增加</button>

// vue2中

render(ctx){
  return createVNode('button', {
    onClick: function(){
  
    }
  })
}

// 在vue3中

render(ctx, _cache){
  return createVNode('button', {
     onClick: _cache[0] || (_cache[0] = () => {} )
  })
}
```



## 4. Block Tree

1. vue2在对比新旧树的时候， 并不知道哪些节点是静态的， 哪些是动态的， 因此只能一层一层的比较， 这就浪费了大部分时间在对比静态节点上

```html
<form>
  <div>
    <label>账号：</label>
    <input v-model="user.loginId" />
  </div>
  <div>
    <label>密码：</label>
    <input v-model="user.loginPwd" />
  </div>
</form>
```

![blocktree1](D:\CL武林秘籍\画图\blocktree1.png)

![blocktree2](D:\CL武林秘籍\画图\blocktree2.png)



## 5. PatchFlag

1. vue2在对比节点时， 并不知道这个节点的哪些相关信息会发生变化， 因此只能将所有信息依次对比

```html
<div class="user" data-id="1" title="user name">
  {{user.name}}
</div>
```

![patchflag](D:\CL武林秘籍\画图\patchflag.png)

