# 高阶组件HOC

1. 高阶组件: 可以利用高阶组件实现横切关注点
2. 用法: 传一个组件给我，我返回一个组件出去，在这中间可以完善其他功能，就真正做到组件干自己的事情就好



```react
export default class Test extends React.Component{
    
}


function TestHoc(component){
    return class Test2 extends React.Component{
        
    }
}


TestHoc(Test);
```





#### 3.注意的点

1. 不要在render函数中使用高阶组件，也不要在函数组件中使用高阶组件
2. 不要在高阶组件的内部修改传入的组件