# 纯组件模式PureComponent

## 1. 为什么会有纯组件模式

1. 因为在有些时候，有些组件并没有发生什么变化，却被重新渲染了
2. **所以我们需要在一个组件的属性和状态都没有发生变化的时候，那么我们就认为这个组件没有必要重新渲染**



## 2. 优化的点

####   1. 类组件

```react
shouldComponentUpdate(nextProps, nextState){
    if(baseEqual(this.props, nextProps) && baseEqual(this.state, nextState)){
        return false;
    }
    
    return true;
}


/**
* 浅比较，只比较第一层的属性
**/
function baseEqual(target1, target2){
    for(let prop in target1){
        if(!Object.is(target1[prop], target2[prop])){
            return false;
        }
    }
    
    return true;
}
```

```react
//第二种方式
直接让那个组件继承PureComponent

class Comp extends PureComponent{
    
}

//这样的方式同样可以达到第一种方式
```



#### 2.函数组件

1. 可以利用高阶组件来包装一下，然后返回一个类组件。