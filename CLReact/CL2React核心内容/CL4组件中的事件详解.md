# 组件中的事件详解

## 1. 定义

1. 在React中，组件的事件本质上就是一个属性，需要使用小驼峰命名法



## 2. 事件出现的场合

```react
//1. 在定义react元素时， 可能会往这个元素身上绑定一个事件属性

const div = (
    <div onClick={ () => {} }></div>
)


//2. 组件之间的事件传递， 在达到某种条件下，一个组件要使用另一个组件传过来的方法，  就是你知道时间点却不知道要干什么，我知道要干什么却不知道时间点
```



## 3. 事件中的this指向

**如果没有特殊处理，在事件处理函数中，this指向undefined**

```react
//1. 为什么this会指向undefined

import B from '';

class A extends React.Component{
    constructor(){
        super();
        
        //1.绑定this来解决
        this.handle = this.handle.bind(this);
    }
    
    handle(){
        
    }
    
    //2.使用箭头函数解决
    handle = () => {
        
    }
    
    render(){
       return 
        <div>
            <B onHandle={ this.handle }></B>
        </div>
    }
}



class B extends React.Component{
    state = {
        
    }
    
    constructor(props){
        super(props);
    }
    
    render(){
        if(){
           this.props.handle();
        }
    }
}

//1. 因为再调用这个方法的时候是this.props调用的，所以this会指向undefined

//2. 解决办法
1. 使用bind函数绑定this
2. 使用箭头函数
```

