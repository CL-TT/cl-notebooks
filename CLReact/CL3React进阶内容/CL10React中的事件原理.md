# React中的事件原理

## 1. 什么是React中的事件

首先他不是真实的DOM事件， **而是React内置的Dom组件中的事件**



## 2. 原理(在document上注册事件)

```js
1. 给document上注册事件，如果在真实dom事件中做了阻止冒泡的操作，就会导致React事件不会执行

2. 几乎所有元素的事件处理，都在document的事件中处理

3. 在document的事件处理中，会根据React的虚拟Dom树来完成事件的调用

4. 为了执行效率，React使用事件对象池来处理事件对象
也就是不同事件中的e是相等的
e === e
```



## 3. 注意

```js
1. 真实Dom的事件执行一定会优先于React事件的执行

2. React事件参数，并非真实的dom的事件参数， 如果在React事件中使用冒泡，他阻止的是React事件，并不会阻止真实dom事件

3. 但是可以通过e.nativeEvent.stopImmediatePropagation()  来阻止document剩余的事件执行

4. 在事件处理程序中，不要异步的使用对象，如果一定要使用，可以先把事件对象持久化， e.persist()
```

