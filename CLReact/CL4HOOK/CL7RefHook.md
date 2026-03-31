# Ref Hook

## 1.Ref Hook的本质

1. useRef(传递一个参数)函数， 返回一个固定的对象，这个对象地址不会发生变化

   { current: 值 }

```react
//1. 就是因为在函数组件里，用createRef这种方式创建的话，每一次函数运行这个ref都会被重新创建，所以在函数组件中更倾向于使用RefHook来创建

function a(){
    const ref = React.createRef();
}


//1. React.createRef和Ref Hook的区别

const ref = React.createRef(); 

const ref = useRef();

Ref Hook 返回的是一个固定的对象，对象的地址不会发生变化
```



## 2.Ref Hook的使用