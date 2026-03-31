# 组件详解

## 1. 初识组件

1. 创建组件时，组件的名称首字母必须要大写，如果不大写的话他会认为是一个普通的react元素，就不会渲染成组件



## 2. 函数式组件

```react
//1. 函数式组件必须要返回一个react元素
//2. 参数props是一个对象，里面是传进来的属性值

function MyFunComp(props){
    return <div></div>
}
```



## 3. 类组件

```react
//1. 必须要继承React.component
//2. 必须要有render()函数， 函数里面必须返回react元素

import react from 'React';

class MyClassComp extends React.Component{
    constructor(props){
        super(props);  //this.props = props
    };
    
    render(){
        return <div></div>
    }
}
```





## 4. 组件的属性

1. 原则上，属性属于谁，谁才有权力改动，**一般传入组件的属性是不允许被改动的**，数据是自顶向下流动的。

2. 组件的属性使用小驼峰命名法

   ```react
   <MyComp num="{3}" obj="{ {age: 20} }"></MyComp>
   ```

3. 函数式组件使用参数props来接收，  类组件在构造函数中的参数props来接收

4. 传递元素内容

   ```react
   <MyComp>
       <div></div>
   </MyComp>
   
   //像这样写的话，在props对象那里会有一个children属性
   ```

   