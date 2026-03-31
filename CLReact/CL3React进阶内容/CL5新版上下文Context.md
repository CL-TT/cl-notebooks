# 新版上下文Context

## 1. 创建上下文

1. 新版创建上下文不像旧版那样依赖于类组件，他是独立于组件的一个对象

```react
const cxt = React.createContext();

<cxt.Provider value={ 可传入的数据 }>
    在这个组件的子组件里面都可以使用cxt上下文中的数据
</cxt.Provider>

这个cxt对象中有两个属性，一个属性是provider另一个属性是Consumer

有一种生产者和消费者的思想在里面
```



## 2. 使用上下文

####   1. 在类组件中使用

```react
static contextType = cxt;

可以直接使用  this.context
```



####   2. 在函数组件中使用

```react
//1. 可以利用cxt的Consumer属性，这个属性就是一个组件

<cxt.Consumer>
    {(value) => {
        return (内容)
    }}
</cxt.Consumer>

//参数value就相当于上下文，可以通过value来使用上下文的数据
```

