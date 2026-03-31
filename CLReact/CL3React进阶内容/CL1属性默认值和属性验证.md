# 属性默认值和属性验证

## 1.属性默认值

1. 如果是类组件

```react
static defaultProps = {
    
}
```



2. 如果是函数式组件

```react
MyFun.defaultProps = {
    
}
```





## 2.属性验证

1. 使用库**prop-types**
2. 对组件使用静态属性  **static propTypes**告知react如何检查属性

```react
static propTypes = {
    n: PropTypes.string.isRequired //属性必须是字符串，并且是必填
}

MyFun.propTypes = {
    
}
```





## 3.验证类型

```react
1. propTypes.string      字符串

2. propTypes.number      数字类型

3. propTypes.any         任意类型

4. propTypes.bool        布尔类型

5. propTypes.func        函数类型

6. propTypes.array       数组类型

7. propTypes.object      对象类型

8. propTypes.symbol      符号类型

9. propTypes.node        任何可以被渲染的内容，比如字符串，数字，react元素

10. propTypes.element     React元素

11. propTypes.elementType  React元素类型

12. propTypes.instanceOf(构造函数)   必须是指定构造函数的实例

13. propTypes.oneOf([a, b])    枚举，传入的值必须是这个数组的一个

14. propTypes.oneOfType([a, b])   属性类型必须是数组中的其中一个

15. propTypes.arrayOf(propTypes.xxx)   必须是某一类型组成的数组

16. propTypes.objectOf(propTypes.xxx)  对象由某一类型的值组成

17. propTypes.shape({
    a: propTypes.string
    b: propTypes.number
})     属性必须是对象，并且满足指定的对象要求

18. propTypes.exact({
    a. propTypes.string
})     属性必须是对象，并且要精确匹配传递过来的数据
```

