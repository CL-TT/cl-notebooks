# React中的渲染原理，或者说是渲染过程

## 1. 名词解析

####    1. React元素

1. React Element 通过React.createElement创建(语法糖 JSX)

   ```react
   const div = <div></div>
         
   <App></App>
   ```

   

####    2. React节点

1. 专门用于渲染到ui界面的对象，React会通过React元素创建React节点，ReactDOM一定是通过React节点来进行渲染的

2. 节点类型

   1.React Dom节点  创建该节点的React元素类型是一个字符串

   2.React组件节点  创建该节点的React元素类型是一个函数或者是一个类

   3.React文本节点  由字符串创建

   4.React空节点  由null，undefined， false，  true

   5.React数组节点  该节点由一个数组创建



## 2. 首次渲染

1. 通过参数的值创建节点

2. 根据不同的节点做不同的事情

   1.**文本节点**，通过document.createTextNode创建真实的文本节点

   2.**空节点**，什么事情都不做

   3**.数组节点**，遍历数组，将数组的每一项递归创建节点（回到第一步进行反复操作，直到遍历结束）

   4.**Dom节点**，通过document.createElement创建真实的DOM对象，然后遍历对应的React元素的children属性，递归操作(回到第一步进行反复操作，直到遍历结束)

   5.**组件节点**，分为函数组件和类组件

   1. 函数组件: 调用函数一定要返回一个可以生成节点的内容，将该函数的返回结果递归生成节点(回到第一步的操作，直到遍历结束)

   2. 类组件

      1.创建该类的实例对象

      2.立即调用生命周期中的getDerivedStateFromProps方法

      3.运行该对象的render方法，拿到节点对象(将该节点进行递归操作，回到第一步操作，直到遍历结束)

      4.**一定是等第三步完成过后，将componentDidMount加入到执行队列中(先进先出)** 当整个虚拟Dom树全部构建完毕，并且将真实的DOM对象加入到容器中(这个容器指代的是什么呢)后，执行该队列

3. **节点生成虚拟Dom树，将该树保存起来，以便后续使用**

4. 将之前生成的真实的DOM对象，加入到容器中去



```react
//实例1 对一个React的结构，他的渲染过程进行解析，  
const app = 
      <div>
          <h1>我是h1标签</h1>
          ['aa', null, undefined, <span></span>]
      </div>
      
ReactDOM.render(app, document.querySelector('#app'));

//这样的一个结构，渲染过程如下
```

![t3](C:\Users\caolei\Pictures\React\t3.png)



```react
//实例2  对组件节点的函数组件进行演示

function A(){
    return (
        <div>
            哈哈
            <B></B>
        </div>
    )
}

function B(){
    return (
        <div>
            ['a', false, <span>啦啦</span>]
        </div>
    )
}

ReactDOM.render(A, document.querySelector('#root'));
```

![t4](C:\Users\caolei\Pictures\React\t4.png)



```react
//实例3  对组件节点的类组件进行演示
class A extends Component{
    render(){
        return (
            <div>
                <B></B>
            </div>
        )
    }
}

class B extends Component{
    render(){
        return (
            <div>haha</div>
        )
    }
}
```

![t5](C:\Users\caolei\Pictures\React\t5.png)





## 3. 更新节点

#### 1.节点什么时候会更新？

1. ##### 重新调用ReactDOM.render()函数，会从根节点开始更新

   **1.进入根节点的对比更新(Diff算法)**

2. ##### 调用类组件中setState，会从该实例节点开始更新

   1.会先运行生命周期函数， static getDerivedStateFromProps

   2.运行shouldComponentUpdate，如果该函数返回false，终止当前流程

   3.运行render函数，得到一个新的节点，**进入该新的节点的对比更新(Diff算法)**

   4.将生命周期函数getSnapshotBeforeUpdate加入执行队列，以待将来执行

   5.将生命周期函数componentDidUpdate加入到执行队列，以待将来执行

3. ##### 不管以上两种哪种方式，都会执行这后续步骤

   1.完成真实的DOM更新

   2.依次调用执行队列中的componentDidMount

   3.依次调用执行队列中的getSnapshotBeforeUpdate

   4.依次调用执行队列中的componentDidUpdate




### 2.对比更新

将新产生的节点，对比之前虚拟DOM中的节点，发生差异，完成更新



这时候就会产生一个问题：不知道对比DOM树中的哪一个节点



React为了提高效率，做出了如下假设

1. 假设节点不会出现层次的移动（对比时，直接找到旧树中的对应位置的节点进行对比）

2. 不同的节点类型会生成不同的节点结构

   2.1 相同的节点类型: 节点本身类型相同，如果是组件节点，组件节点的类型也要相同

   2.2 其他的都是不同的节点类型

3. 多个兄弟通过唯一标识key,来确定对比的新节点



#### 1. 如果找到对比目标

类型一致

1. 空节点，不作任何事情

2. DOM节点

   2.1 直接重用之前的真实DOM对象

   2.2 将其属性的变化记录下来，以待将来统一完成更新（现在不会真正的变化）

   2.3 遍历新的React元素的子元素（递归，对比更新的过程）

3. 文本节点

   3.1 直接重用之前的真实DOM对象

   3.2 将新的文本值记录下来，以待将来统一完成更新

4. 组件节点

   函数组件：直接调用函数，得到一个节点对象，进入递归对比更新

   类组件：

      重用之前的实例对象

      步骤跟setState更新类组件一样

5. 数组节点： 遍历数组，递归对比更新



类型不一致（整体上，全新创建新的节点，卸载旧的节点）

1. 创建新的节点：进入新节点的挂载流程

2. 卸载旧结点

   2.1 文本节点，空节点，数组节点，DOM节点，函数组件节点：直接放弃该节点，如果节点有子节点，递归卸载

   2.2 类组件节点：直接放弃该节点，然后调用生命周期函数componentWillUnMount函数，然后递归卸载

  

#### 2. 没有找到对比目标

新的DOM树中有节点被删除，新的DOM树有节点被添加



#### 3. key值的作用 （用来确定对比的节点）

用于通过旧结点，寻找对应的新节点，如果某个旧结点有key值，则其更新时，会寻找相同层级的相同的key值节点进行比较



数组在循环的为什么要加key值？

就是为了在更新阶段，有key值可以快速的找到（之前的）对比的对象，减少不必要的更新，提高效率