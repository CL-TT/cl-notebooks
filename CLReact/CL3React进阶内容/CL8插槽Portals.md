# 插槽Portals

## 1. React中的插槽是什么

1. **将一个React元素渲染到一个指定的Dom容器中**

```react
<div id="app"></div>
<div id="modal"></div>


class App extends Component{
    render(){
        return (
           <div class="app">
             <ChildA></ChildA>     
           </div>
        )
    }
}

function ChildA(){
    return (
        <div class="A">
            <ChildB></ChildB>
        </div>
    )
}

function ChildB(){
    return (
        <div>组件B</div>
    )
}

//虚拟Dom的结构应该是
App > ChildA > ChildB

//真实Dom结构
<div id="app">
    <div class='app'>
        <div class='A'>
           <div class='B'></div>
        </div>
    </div>
</div>
```

```react 
//上面的结构用插槽进行改变一下
//我想把组件A里面的内容渲染到容器modal中

function ChildA(){
    return ReactDom.createPortal(
        (<div class='A'> <ChildB></ChildB> </div>),
        document.querySelector('#modal')
    )
};

//虚拟的Dom结构并没有发生太大的变化

//真实Dom结构
<div id='app'>
    <div class='app'></div>
</div>
<div id='modal'>
    <div class='A'>
        <div class='B'></div>
    </div>
</div>
```



## 2. 这样利用插槽会出现什么问题

1. 在React中，**事件冒泡是根据虚拟Dom的结构来的**
2. 所以在有事件冒泡的场景下就会出现问题，  就会出现不是我的子元素点击还会出现冒泡事件这种问题。