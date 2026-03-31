# BFC的理解

## 1.什么是BFC

BFC 又称 Box Formatting context(块级格式化上下文) 他是页面中的一块渲染区域，拥有一套自己的渲染规则，

区域里面的元素不会在布局上影响外面的元素。



## 2.怎么触发BFC

```javascript
//1.position: absolute/fixed
//2.float: left/right
//3.display: inline-block
//4.overflow: hidden
```



## 3.BFC能解决哪些问题，BFC的作用

1. margin塌陷问题

   问题描述：一个父元素里面的子元素设置margin-top时不起作用， 前提是父元素没有设置border-top属性

   因为父元素没有了顶部的限制，父子元素共用了垂直方向上同一个top，谁的margin-top大就听谁的

​       解决方法： 让父元素触发BFC

2. 父级元素没有设置高度由子元素撑开，但子元素有浮动，父级元素就包裹不住子元素

   解决方法：让父级元素产生BFC

3. 产生了BFC的元素能看到浮动元素