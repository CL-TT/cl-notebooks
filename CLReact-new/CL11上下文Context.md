# 上下文Context

## 1. 为什么会有上下文

上下文是为了解决组件之间数据共享的问题，可能会说不是有redux能解决这个问题吗，为什么还需要上下文呢，redux它是基于Context实现的，如果你不想使用redux的话，用上下文也能解决组件之间数据共享的问题



## 2. 上下文的使用

第一种方式是使用 context.Consumer

第二种方式使用hook， useContext 

如果有多个上下文，且数据存在同名的情况下，遵循就近原则

```jsx
context/index.js     // 专门用于创建上下文的文件


// 可以有默认值的
// context里面有两个组件一个是Provider提供数据的，另一个是Consumer消费数据的
export const myContext1 = React.createContext({
    name: ''
})


function App() {
    const [ count, setCount ] = useState(0)
    return (
      <myContext1.Provider value={{ count, setCount }}>
        <div>
          <Comp />
        </div>  
      </myContext1.Provider>
    )
}

// 第一种方式
function Comp() {
    return (
        <myContext1.Consumer>
          {
                (context) => {
                   return (
                     <div>{ context.count }</div>
                   )         
                }
           }
        </myContext1.Consumer>
    )
}

// 第二种方式
function Comp() {
    const { count } = useContext(myContext1)
    
    return (
      <div>{ count }</div>
    )
}

// 可以直接使用this.context来获取数据
class Comp extends React.component {
    static contextType = myContext1
    
    render() {
        return (
          <div>
            { this.context.count }  
          </div>
        )
    }
}
```

