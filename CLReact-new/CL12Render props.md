# render props的使用

## 1. 为什么需要render props

render props解决的问题是，当组件有共同的逻辑时，只是页面展示不同，可以把共同的逻辑抽离出来，横切关注点

```jsx
function A() {
    // 逻辑和B完全相同
    
    return (
      <div>展示A组件</div>
    )
}


function B() {
    // 逻辑和A完全相同
    
    return (
      <div>展示B组件</div>
    )
}


function C(props) {
    // 写共同的逻辑
    
    return(
      props.render? props.render({ 写要传入的数据 }) : null
    )
}

<C render={
  (props) => {
    return <A { ...props }/>  
  }        
}/>

<C render={
  (props) => {
    return <B { ...props }/>  
  }        
}/>
```



## 2. render props和高阶组件有什么区别

它们两者都是可以做到横切关注点的，都是可以抽离公共逻辑的，render props着重于两个组件的逻辑完全相同，只是页面展示不同，高阶组件着重于两个组件的逻辑有部分相同，可以有不同的逻辑部分， 只需要抽离出那相同的部分就可以，着重与增强组件的某种功能