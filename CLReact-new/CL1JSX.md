# JSX

```jsx
function app(){
    const name = 'cl'
    return (
        <div>
            <div>{ name }</div>
            <div id={id}></div>
            <div style={{ color: 'red' }} className="wrap"></div>
        </div>
    )
}

这返回的是一个js对象，不是html元素
```



## 1. jsx语法规则

1. 根元素只能有一个
2. jsx中使用javascript表达式，表达式要写在{}中
3. 元素上的属性绑定，也是用{}
4. style对应样式对象， class要写成className



## 2. createElement方法

jsx是一种js的语法扩展， babel会把jsx转译成一个名为React.createElement函数

```jsx
React.createElement(type, [props], [...children])

type: 创建的react元素类型 (可选值有，标签名字符串， react组件)

props(可选): React元素的属性

children(可选): React元素的子元素


// 不同的写法，创建出相同的元素
function createElement(){
    return (
        <div className="wrap">
            nihao
        </div>
    )
}

const element = React.createelement('div', { className: 'wrap' }, 'nihao')
```

**jsx本质上就是React.createElement()的一种语法糖**