# Ref详解

## 1.为什么要使用ref

1. 希望直接使用dom元素中的某个方法，或者直接使用自定义组件中的某个方法
2. **ref能够作用到HTML元素上，这样可以获取到dom元素本身**
3. **ref能够作用到类组件上**，获得的是这个类的实例，可以获取到他的方法
4. **ref不能作用到函数式组件上**



## 2.如何使用ref

1.使用字符串赋值，但这种方式已经不推荐使用

```react
//使用字符串的方式只能在类组件中使用， 函数式组件不能使用这种方式

<div ref="div"></div>

this.div = this.refs.div;
```



2.使用对象的方式

```react
//必须在constructor中创建

this.input = React.createRef();

//这个this.input是一个对象，里面有一个current属性

this.input = {
    current: null
}

<div ref={ this.input }></div>
```



3.使用函数的方式

```react
//1. 直接把函数写在这个元素上

<div ref={ (el) => { this.input = el } }></div>

//这个el参数指代的就是这个元素本身

1. componentDidMount的时候会调用该函数，就是会执行此函数， 也就是说在这个函数中才可以使用ref的值， 先执行那个函数，再执行ComponentDidMount函数

2. 如果ref的值发生了变动(旧的函数被新的函数替代)，会分别调用旧的函数以及新的函数， 时间点会出现在componentDidUpdate之前
  2.1. 旧的函数被调用时， 传递null
  2.2. 新的函数被调用时， 传递对象
  2.3. 如果ref所在的组件被卸载，会调用函数


//2. 把函数写在外面

1. 那么只会在componentDidMount的时候调用一次， 因为每一次传入的函数都是一样的， 并且他会认为你只是想保存一下dom实例
```





## 3.Ref转发

#### 1.为什么要有ref转发

1. ref的作用就是为了防止有些功能需要获取到Dom元素本身，或者获取到组件本身，但获取到组件只有类组件才可以使用ref，函数式组件不能直接使用ref，所以需要使用ref转发

#### 2.Ref转发的使用

```react
1. React.forWardRef() 传入一个组件，只能传入一个函数式组件，返回一个新的组件，功能并没有发生变化

2. A组件可以接受两个参数 props, ref

3. 如果你想获取哪一个Dom元素，就把ref挂载到那个元素上

function A(props, ref){
  return (
    <div ref={ ref } ></div>
  )
}

const newA = React.forwardRef(A)

class App extends Component{
  myRef = React.createRef();

  render(){
    return (
      <div>
        <newA  ref={ this.myRef } />
      </div>
    )
  }
}
```



#### 3.ref转发的流程

1. 先通过React.forwardRef(A)得到一个新的组件

2. 在这个新组件上使用ref = { this.myRef }
3. 但是这个ref并不会指向这个新组件， 会把这个ref给到参数组件A， 作为他的第二个参数
4. 然后可以使用在对应的dom元素上