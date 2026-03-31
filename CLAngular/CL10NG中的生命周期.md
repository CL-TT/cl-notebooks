# NG中的生命周期

## 1. 什么是生命周期

1. 生命周期函数通俗的讲就是组件创建，组件更新，组建销毁的时候会触发的一系列方法



## 2. 生命周期钩子分类

1. #### 指令与组件共有的钩子

   ngOnChanges

   ngOnInit

   ngDoCheck

   ngOnDestroy

2. #### 组件独有的钩子函数

   ngAfterContentInit

   ngAfterContentChecked

   ngAfterViewInit

   ngAfterViewChecked



## 3. 生命周期钩子函数详解

```ts
1. constructor 组件初始化

2. ngOnChanges 当数据绑定输入属性的值发生变化时调用 (没有绑定数据时，这个生命周期函数不会触发)， 第一次绑定的时候就会调用

3. ngOnInit 在第一次ngOnchanges后调用， 一般请求数据放在这里面

4. ngDoCheck 用于检测和处理值的改变， 他发生在angular无法或者不愿意自己检测的变化时做出反应  (子组件自己的属性发生改变， 该函数会触发， 子组件触发函数让父组件传过来的属性发生变化，该函数同样会发生变化)

5. ngAfterContentInit 当把内容投影进组件之后调用，第一次ngDoCheck之后调用， 只会调用一次

6. ngAfterContentChecked  组件每次检查内容时调用， 也就是每次完成组件内容的变更监测之后调用 (子组件自己的属性发生变化，该函数会触发， 子组件触发函数让父组件传过来的属性发生变化，该函数同样会发生变化)

7. ngAfterViewInit  组件相应的视图以及子视图初始化之后调用(Dom真正被渲染了)， 在这里面可以操作Dom

8. ngAfterViewChecked  每次做完组件视图和子视图的变更检测之后调用  (子组件自己的属性发生变化，该函数会触发， 子组件触发函数让父组件传过来的属性发生变化，该函数同样会发生变化)

9. ngOnDestroy  组件每一次销毁时调用， 通常移除事件监听， 清除定时器  
```

