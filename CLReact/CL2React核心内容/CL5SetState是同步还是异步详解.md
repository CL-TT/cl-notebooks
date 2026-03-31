# SetState是同步还是异步

## 1. setState到底是同步还是异步

1. setState对于状态的改变， **可能是异步的**
2. **如果改变状态的代码处于某个HTML元素的事件中，则其是异步的，否则是同步的**



```react
import React { Component } from 'react';
export class Comp extends Component{
    state = {
        n: 0
    };

    add = () => {
        this.setState({
            n: this.state.n + 1
        });
        
        console.log(this.state.n);
    }

    render(){
        return (
            <div onClick={ this.add }></div>
        )
    }
}

//点击增加按钮时， 会打印 0， 1， 2 ...,     而不是打印1， 2， 3 ...,    所以可想而知在这个地方setState是异步的
```



## 2. 想要获取到改变状态之后的值

```react
//如果想要获取到改变状态之后的值，就需要在setState的第二个参数中，在回调函数中获取

this.setState({
    n: this.state.n + 1
}, () => {
    console.log(this.state.n);
})

//这个地方打印就会打印 1， 2， 3
```





## 3. 如果同步调用多次setState,会发生什么呢

#####   1. 在同步的情况下该怎么样就怎么样

#####   2. 在异步的情况下

```react
this.setState({
    n: this.state.n + 1
})

this.setState({
    n: this.state.n + 1
})

this.setState({
    n: this.state.n + 1
})

//n的值并不会变成3， 因为这是异步的，所以执行过后结果还是1
```



## 4. 新的状态要根据之前的状态进行运算

1. 这种情况可以使用setState的第一个参数来解决， 他的第一个参数可以是函数， 当然这个函数也是异步的，**但是这个函数的参数，当前状态值，他是可以信任的**

```react
this.setState((current) => {
    return {
        n: current.n + 1
    }
    //返回值会覆盖掉state中的值
})

this.setState((current) => {
    return {
        n: current.n + 1
    }
    //返回值会覆盖掉state中的值
})

this.setState((current) => {
    return {
        n: current.n + 1
    }
    //返回值会覆盖掉state中的值
})
//这样n的值就会变成3
```



## 5. 异步的setState的思路原理

**React会对异步的setState进行优化， 将多次的setState进行合并(将多次状态改变完成后，再统一对state进行改变， 然后触发render， render只会触发一次)**

```react
this.setState((cur) => {
    return {
        n: cur.n + 1
    }
}, () => {
    console.log(this.state.n);
    
    //这个地方打印的n的值不是1， 而是3，   会在状态都改变完成后，并且render函数也执行完后，才会执行这句话
})

this.setState((cur) => {
  return {
      n: cur.n + 1
  };
})

this.setState((cur) => {
  return {
      n: cur.n + 1
  };
})
```

**思路： 他认为一般事件处理都会是HTML元素的事件处理， 一般这种事件处理都会是很复杂的， 比如说有一个按钮触发了他的点击事件，这个事件中要处理10个事件， 这些事件分布到10个函数中，如果每一个函数中都有状态改变那么改变一次就会重新渲染一次，及其消耗性能。 干脆把这些状态改变合并并做成异步并放入到队列中， 然后从队列中一个一个的取出来执行， 全部执行完之后， 再对状态进行改变。**





## 6. setState的最佳实践

1. **把所有的setState都当成是异步的**
2. **永远不要信任setState调用之后的状态**
3. **如果想要使用改变之后的状态，需要使用回调函数（setState的第二个参数）**
4. **如果新的状态要根据之前的状态进行运算，使用函数的方式改变状态（setState的第一个参数）**