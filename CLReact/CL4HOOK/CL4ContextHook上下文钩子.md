# ContextHook上下文钩子

```react
const cxt = React.createContext();

function App(){
    return (
        <div>
            <cxt.Provider value={}>
                <Test></Test>
            </cxt.Provider>
        </div>
    )
}

function Test(){
    //不用hook时的使用上下文
    return (
        <div>
            <cxt.Consumer>
                { (cxt) => { <h1>上下文的数据： { cxt.name } </h1> } }
            </cxt.Consumer>
        </div>
    )
    
    
    //使用Hook的情况
    const cxt = useContext();   //直接获取到上下文对象，可以直接使用的
    
    return (
        <h1>上下文的数据：{ cxt.name }</h1>
    )
}
```

