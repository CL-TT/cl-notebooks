# ImperativeHandle钩子

## 1.这个钩子的作用

1. 我们都知道ref只能用在类组件上，不能使用在函数组件上，但是有了ref转发，同样可以使用在函数式组件上，但是ref在类组件上可以使用类中的方法，但是函数却不能。
2. 所以这个钩子就是解决这个问题的，可以调用函数式组件中的方法



## 2.这个钩子的使用

1. 如果不给依赖项，则每次运行函数组件都会调用该方法
2. 如果使用了依赖项，则第一次调用之后，会进行缓存处理，只有依赖项发生变化时才会重新调用

```react
//1. 基本使用
useImperativeHandle(ref, () => {
    return {
        method: () => {
            
        }
    }
    
    返回什么current的值就是什么
    
    就相当于 ref.cuurent = {
        method: () => {}
    }
})



//2. 例子
function A(props, ref){
    useImperativeHandle(ref, () => {
        return 1
    })
    
    return (
        <div></div>
    )
}

const NewA = React.forwardRef(A);

class App extends Component(){
    state = {};

    myRef = React.createRef();

    componentDidMount(){
        console.log(this.myRef.current)  //会打印1
    }
    
    return (
        <div>
            <NewA ref={ this.myRef }></NewA>
        </div>
    )
}
```

