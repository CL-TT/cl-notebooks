# 旧版上下文Context

## 1.什么是上下文Context，以及上下文Context的作用

1. Context通过组件树提供了一个传递数据的方法，从而避免在每一个层级都要手动的传递数据
2. 当用props属性进行组件向下传值的操作时，遇到多个组件进行嵌套，也许只是最后一个组件需要那个值，可是这中间的组件却被迫接收了传过来的值
3. 所以需要另外一种数据传递的方式，避免在每一个层级都要手动的进行props传值



## 2.Context的基本使用

```react
//1. 先约束要使用的上下文变量类型
static childContextTypes = {
    name: propTypes.string,
    age: propTypes.number,
    func: propTypes.func
}

//2. 得到上下文的值
getChildContext(){
    return {
        name: this.state.name,
        age: 20,
        func: () => {
            
        }
    }
    
    //这个函数运行在render函数后面
}

//3. 使用
在其他组件中使用

static contextTypes = {
    name: propTypes.string
}

constructor(props, context){
    super(props, context);
    
    this.context = context;
}
```



## 3.Context上下文值的改变

1. 在子组件中想要改变context的值，可以像改变属性值那样，通过调用传过来的方法进行改变

   

## 4.旧版上下文要注意的点

1. 如果一个组价身处多少个上下文中，一定是趋于就近原则。

2. 旧版API存在严重的效率问题，并且容易导致滥用。