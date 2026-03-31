# 理解RequestAnimationFrame

## 1. 含义

requestAnimationFrame是浏览器提供的一种用于动画的API，他在下一个动画帧准备好时执行回调函数， 这个方法可以用来替代setTimeout和setInterval



## 2. 使用

```js
function animate(time) {
    // 动画逻辑
    
    requestAnimationFrame(animate)  // 是一个递归的过程
}

const rid = requestAnimationFrame(animate)

cancelAnimationFrame(rid)  // 取消动画
```



## 3. 使用的好处

1. 使用setInterval 实现的动画，当页面被隐藏或最小化时，setInterval 仍然在后台执行动画任务，由于此时页面处于不可见或不可用状态，刷新动画是没有意义的，完全是浪费CPU资源。而RequestAnimationFrame则完全不同，当页面处于未激活的状态下，该页面的屏幕刷新任务也会被系统暂停，因此跟着系统步伐走的RequestAnimationFrame也会停止渲染，当页面被激活时，动画就从上次停留的地方继续执行，有效节省了CPU开销
2. 与setInterval 相比，RequestAnimationFrame最大的优势是由系统来决定回调函数的执行时机**，**具体一点讲，如果屏幕刷新率是60Hz，那么回调函数就每16.7ms被执行一次, **能保证同步，就不会出现丢帧的现象**