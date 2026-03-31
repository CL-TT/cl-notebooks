# 表单

## 1. 受控组件

1. 受到组件中自己有的状态数据（state）控制， 比如在input元素上绑定value， 或者单选框元素上绑定check, 此类被称为受控组件

```react
class Forms extends Component{
    state = {
        value
    }

    handleChange = (e) => {
        this.setState({
            value: e.target.value
        })
    }
    
    render(){
        return (
            <input value={this.state.value} onChange={ this.handleChange }></input>
        )
    }
}
```



## 2. 非受控组件

1. 此类元素一般都是不受状态数据控制， 要想获取元素的value，一般需要绑定ref