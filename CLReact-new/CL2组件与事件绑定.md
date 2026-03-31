# 组件与事件绑定

## 1. 函数组件与类组件

### 1.1 函数组件

```jsx
function App(){
    return (
        <div>hello world</div>
    )
}
```



### 1.2 类组件

```jsx
class App extends React.Component{
    render(){
        return (
            <div>类组件</div>
        )
    }
}
```



## 2. 事件绑定

```jsx
function App(){
  const submit = (name) => {
      console.log('哈哈', name)
  }  
    
  return (
      <div onClick={ () => submit('name') }></div>
  )
}
```

