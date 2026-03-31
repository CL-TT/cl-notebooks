# RenderProps

## 1.为什么会有render props

1. 当遇到，某些组件的各种功能以及处理逻辑几乎完全相同时，只是显示的界面不一样，可有如下两种处理方式



## 2.解决方式

1. #### render props

   1.某个组件需要某个属性

   2.该属性是一个函数，函数的返回值用于渲染

   3.函数的参数会传递为需要的数据

   4.这个属性一般为render

```react
function A(){}

function B(){}

//组件A和组件B的各种功能和处理逻辑完全相同，只是显示的内容不同

class C extends PureComponent{
    render(){
        return (
            { this.props.children(参数) }
            { this.props.render(参数) }
        )
    }
}

//使用的时候
<C render={ this.方法 }>
    {(参数) => {
        return 不同的内容
    }}
</C>
```



#### 2.HFC高阶组件