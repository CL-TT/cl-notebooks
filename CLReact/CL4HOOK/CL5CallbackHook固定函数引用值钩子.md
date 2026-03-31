# CallbackHook固定函数引用值钩子

## 1.什么是callbackhook，useCallback

1. useCallback，用于得到一个固定引用值的函数，通常用它进行性能优化



## 2.useCallback的使用

1. 该函数有两个参数
2. 第一个参数，传入一个函数，只要依赖项没有发生变化的，则始终返回之前函数的地址
3. 第二个参数，是一个数组，记录依赖项
4. 这个useCallback的返回引用相对固定的函数地址

```react
const callback = useCallback(() => {
    //处理逻辑
}, [依赖项] );
```

