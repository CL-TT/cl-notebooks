# LayoutEffectHook

## 1.什么是LayoutEffectHook

1. 当在useEffect里要操作DOM(真实Dom)时，可以在useLayoutEffect里完成，否则可能会出现闪屏，useLayoutEffect里的callback函数会在DOM更新完成后立即执行，但是会在浏览器绘制之前完成



## 2.LayoutEffectHook的使用

```react
//1. 他和useEffect的区别

1. 运行的时间点不同
2. 使用useEffect不会造成渲染阻塞， 而LayoutEffect则相反会造成渲染阻塞

1. 在Dom进行改动时，这个时候是同步的 =>  2. 浏览器下一次渲染时间点到达，对比差异进行渲染，用户已经看到新效果，这个时候是异步的
=>  3. 如果再从useEffect对真实Dom进行改动，则会出现闪动的问题。

而使用LayoutEffect钩子函数的话，他会在对比差异渲染之前就会执行，这样就避免了闪动的问题，他的运行时间点有点像类组件中的生命周期函数componentDidMount和componentDidUpdate


//2. 操作真实Dom的副作用尽量用LayoutEffect
```

