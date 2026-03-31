# StateHook状态钩子

## 1.State Hook是什么，有什么作用

1. **State Hook是一个在函数组件中使用的函数(useState)，作用是在函数组件中使用状态**



## 2.useState的理解

```react
function A(){
    const [ num, setNum ] = useState(0);
}

//1. useState是一个函数，函数的返回值是一个数组，该数组一定包含这两项
//2. 第一项：当前状态的值
//3. 第二项：改变状态的函数
//4. 第一次传入函数的值就是状态的默认值
```





## 3.useState的原理

```react
import React, { useState } from 'React';

export default function StateHook(){
    const [num, setNum] = useState(0);
    
    const [isShow, setIsShow] = useState(true);
    
    return (
        <div>
            <div style={{ display: isShow? 'block' : 'none' }}>
                <span>{ num }</span>
                <button onClick={ () => { setNum( num + 1 )} } >增加</button>
                <button onClick={ () => { setNum( num - 1 )} } >减少</button>
            </div>
            
            <button onClick={ () => { setIsShow( !isShow )} }>显示与隐藏</button>
        </div>
    )
}
```



**过程**

1. 组件节点StateHook => React Element => 执行function StateHook

2. 第N次调用useState => 会先生成一个状态数组附着在组件节点上 => 检查该节点的状态数组是否存在下标N 

   => 如果不存在的话，会生成一个下标为0，值为0的状态， 接下来又执行了useState， 又生成了一个下标1，值为true的状态

3. 然后想改变状态的值时，点击增加调用setNum()函数，会改变状态数组对应的值，然后又重新调用函数，又重新执行useState, 这次重新执行又会重新计算useState的执行次数，又会检查状态数组是否存在下标N，存在的话不管默认值直接把状态数组的值返回，这时候返回的值已经是被改变过后的值



## 4.需要注意的点

1. useState最好写在函数的起始位置，便于阅读
2. useState严禁出现在代码块中(判断， 循环)中， 因为会导致状态数组形成错乱
3. useState返回的函数(数组的第二项)，引用不变(节约内存空间)
4. 使用函数改变数据，**若数据和之前的数据完全相等(使用Object.is比较)，不会导致重新渲染，以达到优化效率的目的**
5. 使用函数改变数据，传入的值不会和原来的数据进行合并，而知直接替换， 所以对数组和对象要格外慎重
6. 如果要强制刷新的话，类组件使用forceUpdate函数， 函数组件，使用一个空对象的useState
7. **如果某些状态之间没有必然联系，应该分化为不同的状态，而不要合并在一个对象中**
8. 和类组件的状态一样，函数组件中改变状态可能是异步的(在Dom事件中)，多个状态变化合并以提高效率，此时，不能信任之前的状态，而应该使用回调函数的方式改变状态，如果状态变化要使用到之前的状态，尽量传递函数