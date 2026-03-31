# 深入了解Computed

## 1. computed和methods基本的区别

1. computed是被当做属性来使用的， 而methods是被当做方法来使用的
2. computed是有缓存的，而methods是没有的
3. computed中有getter和setter，是可以设置值的， 而methods则不行
4. computed不能传递多个参数，而methods是可以的



## 2. 深入Computed的原理

```vue
<div id="app">
    <div>
        {{ name }}
    </div>
    <div>
        {{ name }}
    </div>
</div>

<script>
    const app = new Vue({
        el: '#app',
        data: {
            firstName: '曹',
            lastName: '磊'
        },
        computed: {
            name: {
                set(){
                    
                },
                
                get(){
                    return this.firstName + this.lastName
                }
            }
        }
    })
</script>
```

1. vue对methods的处理比较简单， 拿到methods配置之后遍历配置中的每一个属性，然后每一个方法通过bind绑定到实例中
2. vue对computed的处理比较复杂， 拿到computed配置之后，遍历配置中的每一个属性，然后给每一个属性创建一个Watcher，Watcher是一个对象，并且会传入一个函数，这个函数的本质就是计算属性的getter方法，在getter的运行过程中就会收集依赖
3. 和渲染函数不同，计算属性创建的Watcher并不会立刻执行，因为他要考虑到是否被使用，如果没有被使用那么就不会运行，因此创建Watcher的时候，他使用了一个lazy配置，Lazy配置可以让Watcher不会立即执行
4. 受到lazy配置的影响，Watcher内部会有两个关键属性，value，和dirty
5. value属性的值正是Watcher运行的结果，受到lazy的影响，value的初始值为undefined
6. dirty属性的值是一个布尔值，表示value的值是否是一个脏值，value的值是否已经过期，受到lazy的影响，dirty的初始值是true
7. Watcher创建好后，会通过代理的方式挂载到vue实例上
8. 当页面上有用到计算属性name,那么就会调用Object.defineProperty中的get方法，他会先判断Watcher.dirty是否为true， 如果为true证明当前值是过期的，所以需要调用Watcher，一调用就会运行计算属性的getter方法计算值赋给value属性然后把dirty属性设为false， 如果不是true就不会调用
9. 后面如果同样用到了计算属性name, 会判断Watcher.dirty,   这个时候已经是false了，就直接把value属性返回，所以getter只会执行一次
10. 当我们改变了计算属性的依赖值，vue做了更巧妙的设计，被依赖的数据不仅会被收集到计算属性的Watcher, 还会被收集到组件的Watcher,  因此当计算属性的依赖值被改变，它会先执行计算属性的Watcher在这一步的操作只是把dirty的属性值设为true然后结束
11. 再执行组件的Watcher, 因此组件会被重新渲染，重新渲染时又读到了计算属性，由于计算属性的dirty为true，所以重新运行getter函数， 计算结果然后赋值给value属性