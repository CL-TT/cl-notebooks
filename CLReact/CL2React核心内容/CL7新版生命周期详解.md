# 新版生命周期

1. **React >= 16.0.0**

## 1.初始化阶段

1. #### constructor 构造函数

   1.只会执行一次

   2.不可以使用setState



## 2.挂载阶段

   1. #### static   getDerivedStateFromProps  从属性中获取新的状态， 不能使用setState

      1.通过参数可以获取到新的属性和状态

      **2.该函数是静态的**

      **3.该函数的返回值会覆盖掉组件的状态**

      4.该函数几乎是没有什么用的

   2. render  返回一个虚拟Dom, 把虚拟Dom挂载到虚拟Dom树上

   3. componentDidMount   挂载完成，最终渲染到真正的页面中

      

## 3.更新阶段

1. #### static  getDerivedStateFromProps   不管是属性还是状态的改变，都会执行这个函数，

2. #### shouldComponentUpdate  是否应该重新渲染

3. #### render  返回一个虚拟Dom， 把虚拟Dom挂载到虚拟Dom树中

4. #### getSnapshotBeforeUpdate(preProps, preState)  获取更新前的快照

   1.真实的Dom构建完成，但还未实际渲染到页面中

   2.在该函数中，通常用于实现一些附加的dom操作

   3.该函数的返回值，会作为componentDidUpdate的第三个参数

   4.这个钩子函数一般会和componentDidUpdate一起使用

5. #### componentDidUpdate (preProps, preState, snap) 虚拟Dom已经重新挂载到页面中成为真实Dom



## 4.销毁阶段

1. #### componentWillUnmount   

   1.可以在这个函数中，销毁一些组件依赖的资源，比如一些定时器和计时器

   

   

   ![t2](C:\Users\caolei\Pictures\React\t2.png) 







