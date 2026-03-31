# React的前世今生

## 1.React的简介

1. React起源于FaceBook, React本来是用与解决 UI复杂度的开源javascript库
2. yarn create react-app react-project
3. npx create-react-app react-project   创建react项目
4. yarn create react-app 名称 --template typescript    创建react项目，并且有typescript

## 2.React的优点

1. 轻量级: 源码加注释也只有三千多行
2. 原生：React基本是完全依赖于原生js
3. 易扩展
4. 不依赖于宿主环境
5. 渐进式：其他项目可以一步步的改成React项目，可以和很多其他的库相兼容
6. 单向数据流



## 3.React起步

#### 1. 要引入三个核心文件

1. 是react的核心功能

<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
2. 是把react和页面结合起来的库，是依赖于react的核心库

<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
3. 把jsx语法转化成js语法的babel.js

<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
​    

#### 2. 创建元素的两种方式

1. 第一种最底层的方式

```react
<div id="div"></div>

//1. 创建一个react元素对象，是虚拟Dom， 不是真正的Dom元素
const span = React.createElement('元素名称', {
    元素属性
}, 元素内容);

//2. 把React元素对象渲染成真正的Dom元素
//第一个参数渲染什么， 渲染到什么地方去
ReactDOM.render(span, document.getElementById('div'));
```



2. jsx的方式

```react
<div id="div"></div>


//用jsx的script标签的属性type="text/babel"

//这是jsx写法,   这个span同样是一个react元素对象
const span = <span class="span">哈哈</span>
      
ReactDOM.render(span, document.querySelector('div'));
```

