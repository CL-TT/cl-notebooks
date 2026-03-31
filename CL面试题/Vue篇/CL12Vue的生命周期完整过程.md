# Vue生命周期的完整过程

![life](D:\CL武林秘籍\画图\life.png)

```
1. new Vue实例被创建，初始化一些生命周期函数

2. 【第一个钩子函数 beforeCreate 执行】, 这个时候数据和方法都没有创建，访问不了

3. 进入注入流程，处理属性，比如data,methods,computed,等   可以讲响应式原理的第一个阶段 原始的对象通过Observer(target)构造函数，把原始的对象变成响应式对象，然后在代理到vue实例上

4. 【第二个钩子函数 created 执行】，这个时候是可以访问数据和方法的，在这个钩子函数里可以做一些数据初始化的请求

5. 判断实例有没有el这个属性，如果有的话，再看有没有template这个属性， 如果有template就把这个模板放入到render函数中， 没有模板有el,就找el关联的元素作为模板，  如果没有el就看实例的$mount属性有没有绑定值，  template >  el  >  $mount    总之把模板放入到render函数中

6. 【第三个钩子函数 beforeMount 执行 】

7. 这个时候可以讲响应式原理的第二个阶段，  render函数并不是直接执行， 他会先创建一个Watcher，Watcher是一个对象，  传入一个updateComponent的函数，该函数会运行render函数，render函数的返回值是一个虚拟Dom节点， 然后会运行_update函数，render函数的返回值作为参数，  在运行render函数的时候，依赖的属性会产生一个Dep,去收集所有的依赖(也就是Watcher)， 当后面依赖发生变化时，又会重新运行updateComponent函数， 在运行_update函数的过程中，会触发__patch__函数的运行， 由于这是第一次渲染， 没有旧树，所以为当前的虚拟Dom树的每一个节点生成一个elm属性，即为真实Dom。 如果遇到创建一个组件的vnode，则会进入组件实例化流程，该流程和创建vue实例流程基本相同，最终会把创建好的组件实例挂载vnode的`componentInstance`属性中，以便复用。

8. 【第四个钩子函数 Mounted 执行】 在这个钩子函数中可以访问真实的Dom

9. 当数据data发生改变时，所有的依赖该数据的Watcher均会重新运行，Watcher也不是自己运行， 他会把自己交给调度器，调度器维护一个队列，相同的Watcher只会在队列里出现一次， 在合适的时机实例方法nextTick会把合适的Watcher放入到事件循环的微队列中， 这样做的目的就是为了避免多个依赖的数据同时发生变化从而导致多次执行Watcher

10. 【第五个钩子函数 beforeUpdate 执行】

11. diff算法那一段流程，  updateComponent函数重新被执行，在执行render函数时，会去掉之前的依赖，重新收集新的依赖， 在执行_update函数时，触发__patch__函数，发现有旧树，和新树进行diff比较
可以讲diff比较的过程， 根节点相同， 在比较子节点。。。

12. 【第六个钩子函数 updated 执行】 页面和data中的数据已经同步

13. 【第七个钩子函数 beforeDestroy】 vue实例已经从运行阶段到销毁阶段，但是数据还是可以用的

14. 【第八个钩子函数 destroyed】 组件完成被销毁，数据等都不可用
```

