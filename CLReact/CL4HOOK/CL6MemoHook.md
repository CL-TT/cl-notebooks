# MemoHook

## 1.什么是MemoHook

1. Memo Hook用于保持一些比较稳定的数据，通常用于性能优化
2. **如果React元素本身的引用没有发生变化，一定不会重新渲染**



## 2.使用MemoHook

```react
//1. MemoHook和CallBackHook的区别

const callback = useCallback(() => {
    //主体内容
}, []);

useCallback的主体就是一个函数，就是第一个参数， 如果依赖项没有发生变化， 那么就直接返回这个函数的引用地址


const callback = useMemo(() => {
    return () => {
        //主体内容
    }
}, []);

1. useMemo的主体是第一个参数的返回值， 如果依赖项没有发生变化，就不会运行useMemo传递的第一个函数，就直接返回上一个返回的结果

2. 并且useMemo可以返回任何东西，不仅仅局限于函数


//2. useMemo有点像计算属性
```

