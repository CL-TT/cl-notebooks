# CSS和HTML汇总

##### 1. 常见标签，有什么区别？（考查的是**行级元素**与**块级元素**的区别）

回答： 元素可以分为三种，行级元素，块级元素， 行级块元素

1. 行级元素的特点是，**不可以设置宽高，不能独占一行**，常见的行级元素有span, strong, a, b, em标签等

2. 凡是带有inline特性的元素， 都带有文字特性，就会留有空隙  一般为8px ，font-size: 0来解决

3. 在行内元素中有几个特殊的标签(既有行级元素的特点又有块级元素的特点，可以设置宽高但又不会独占一行)，比如说img，input， td

4. 块级元素的特点是，**块级元素独占一行，可以为其设置宽高属性** ，常见的块级元素，div, p, ul, li, h1-h6, dl, dt, dd



##### 2. 对盒模型的理解（考查的是两种盒模型的概念）

回答： 可以从什么是盒模型，到分几种盒模型， 到这几种盒模型的区别， 哪个属性决定是什么盒模型来回答

1. css中所有的HTML元素都可以看成是一个盒子，一个盒模型包括外边距margin， 边框border， 内边距padding， 内容content这四个部分

2. 盒模型分为两种，第一种叫做标准盒模型，第二种叫做怪异(也叫IE)盒模型

3. 标准盒模型  

   给盒子设置width时:    width = content区域的宽高

   盒子真正的宽高：width + padding + border + margin

   怪异盒模型

   给盒子设置width时： width =  content + padding + border区域的宽高 + margin

   盒子的真正宽高：width

4. 可以通过属性box-sizing来决定使用什么盒模型， content-box: 标准盒模型， border-box: 怪异盒模型， inherit: 继承父元素的box-sizing的值



##### 3. BFC的理解

回答：可以从什么是BFC，怎么触发BFC，可以用BFC解决什么样的问题来回答

1. BFC 又称 Box Formatting context(块级格式化上下文) 他是页面中的一块渲染区域，拥有一套自己的渲染规则，区域里面的元素不会在布局上影响外面的元素

2. ```
   1. position: absolute / fixed
   2. float: left / right
   3. display: inline-block
   4. overflow: hidden
   
   当然你要为触发bfc的这些属性的副作用买单
   ```

3. 解决什么问题你可以举两个例子

   ```
   1. margin塌陷问题
   
   问题描述：一个父元素里面的子元素设置margin-top时不起作用， 前提是父元素没有设置border-top属性
   
   因为父元素没有了顶部的限制，父子元素共用了垂直方向上同一个top
   
   解决方法： 让父元素触发BFC
   
   2. 父级元素没有设置高度由子元素撑开，但子元素有浮动，父级元素就包裹不住子元素
   
   解决方法：让父级元素产生BFC，产生了BFC的元素能看到浮动元素
   ```



##### 4. 有哪些方式可以让元素水平垂直居中

```css
1. 知道元素的宽高

div{
   width: 200px;
   height: 200px;
   position: absolute;
   left: 50%;
   top: 50%;
   margin-left: -100px;
   margin-top: -100px; 
}

2.不知道元素的宽高

div{
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}

// 这种知道宽高也适用
div{
    position: absolute;
    margin: auto;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0
}

3.第三种利用flex布局
这个元素必须要有父元素，并且父元素要有宽高
设置父元素的  justify-content: center   align-items: center
```



##### 5. 谈谈你对position的理解（考查你对定位的理解，定位有哪些属性以及对属性的理解）

```
1. fixed: 固定定位

元素的位置相对于浏览器窗口是固定的，即使窗口是滚动的他也不会移动， fixed定位使元素的位置与文档流无关就是会脱离文档流，因此不占据空间， 会和其他元素重叠

2. relative: 相对定位

如果元素使用相对定位，他会出现在它所在的位置，进行移动时，元素任然占据原来的空间，移动元素会导致会覆盖其他元素

3. absolute: 绝对定位

绝对定位的元素的位置相对于最近的已定位的父元素， 如果元素没有已定位的父元素，那么他的位置相对于HTML， absolute会使元素脱离文档流，因此不占据空间， 定位的元素会和其他的元素重叠

4. sticky: 粘性定位

元素先按照普通文档流定位，然后如果元素脱离了可视区域，他又会像固定定位那样相对于浏览器窗口进行定位

5. static: 默认值没有定位

默认值，没有定位，元素出现在正常的流中(忽略top, bottom, left, right或者z-index的声明)

6. inherit: 从父元素继承position属性的值
```



##### 6. H5语义化的作用

```
回答：可以从什么是HTML语义化， 为什么要语义化， 增加了哪些语义标签， 这几个方面来回答

1. 可以通过标签来判断内容语义，并让合适的标签去做合适的事情，就叫做标签语义化。

2. 去掉或者样式丢失的时候页面还是能呈现清晰的结构
   方便其他设备解析(比如屏幕阅读器， 移动设备， 盲人阅读器)
   有利于SEO, (面试官可能会问， 你了解SEO吗？)
   便于团队开发和维护，因为语义化更具有可读性，让阅读源码的人更容易将网站分块
   便于理解和维护
   
   
3.什么是SEO(更详细的话，可以自己百度了解， 我也不是很清楚， 嘿嘿)
seo：Search Engine Optimization 搜索引擎优化
在了解搜索引擎排名机制的基础上，对网站进行调整和优化，改进网站在搜索引擎中的关键词排名以获得更多的流量

4.常见的语义化标签

header，footer, nav, main, aside, article
```



##### 7. flex: 1有哪几个属性构成  (考查flex布局，以及对其属性的认识)

```
1. flex: 1, 是由flex-grow, flex-shrink, flex-basis这几个属性组成


2. flex具体属性了解

父元素里面的属性

1. flex-direction: 主轴的方向   默认是row

row: 横向从左到右
row-reverse: 横向从右到左
column: 纵向从上到下
column-reverse: 纵向从下到上

2. flex-wrap: 是否换行   默认是no-wrap

wrap: 换行
no-wrap: 不换行
wrap-reverse: 颠倒换行 

比如   
1 2 3
4 5 6
换行后
4 5 6
1 2 3


3. justify-content: 基于主轴的对齐方式   默认是flex-start

flex-start: 基于主轴的首部对齐
flex-end: 基于主轴的尾部对齐
center: 基于主轴的中心对齐
space-between: 两边不留空，然后子元素之间平分剩余空间
space-around: 两边留子元素与子元素之间距离的一半
space-evenly: 子元素之间和子元素和两边的距离都是一样的


4. align-items: 基于交叉轴的对齐方式   默认是stretch

flex-start: 基于交叉轴的首部对齐
flex-end: 基于交叉轴的尾部对齐
center: 基于交叉轴的中心对齐
baseline: 基于子元素的内容对齐
stretch: 如果子元素没有设置高度，使用了stretch那么就会把子元素变成和父元素一样高


5. align-content: 基于交叉轴的对齐方式，多行才会起作用   默认stretch

flex-start: 把多行的子元素看成一个整体然后基于交叉轴的首部对齐
flex-end: 把多行的子元素看成一个整体然后基于交叉轴的尾部对齐
center: 把多行的子元素看成一个整体然后基于交叉轴的中心对齐
space-between:
space-around:
stretch:


子元素里面的属性

6. order: 决定子元素的排列顺序  值越小越排在前面  默认值为0

7. align-self: 决定子元素自己在交叉轴的对齐方式， 他的权重是要比父元素设置的align-items要高的，但是要比align-content的权重低， 他的属性值和align-items差不多

8. flex-grow: 按比例瓜分剩余的空间   默认值是flex-grow: 0

比如说父元素的宽度为800，  有三个子元素每一个子元素的宽度都为200，总宽度为600，  还剩余200的空间
设置flex-grow  1:1:2,  单独设置的。  就会把200分成4份一份50  50:50:100,  那么三个子元素的宽度就是250,250,300

9. flex-basis: 可以重新自定义子元素的宽度，并且权重比子元素的width要高，可以覆盖width
   
   1. 只设置了flex-basis, 没有设置元素的width， 那么元素的最小宽度就是flex-basis的值，如果元素里面的内容很多就会把元素撑开
   2. 既设置了flex-basis, 又设置了元素的width,  flex-basis会决定元素宽度的下限， width会决定元素宽度的上限， 撑开的最大宽       度就是width的值

10. flex-shrink: 
    压缩算法, 当子元素的宽度超过了父元素，就会进行压缩{
        压缩算法   (width1 * shrink / width1 * shrink1 + width2 * shrink2 + ... ) * 多出的空间
    }

    1. 先算出总的加权值,   这个宽度是元素的内容区的宽度
    const all = width1 * shrink1 + width2 * shrink2 + width3 * shrink3

    2. 再算出每一个元素要缩小的比例
    const bl1 = width1 * shrink1  /   all
    
    3. 再把这个比例 * 多出来的空间
    const yjspace = bl1 * 多出来的空间
    
    4. 再把这个元素的宽度减去  要减去的空间
    const relWidth = width1 - yispace
```



##### 8. 元素隐藏的三种方式的区别

```
1. display: none

元素隐藏之后不会占据位置， 元素的宽高和其他属性都会失效

2. visibility: hidden

同样是隐藏元素， 但元素隐藏之后会占据着原来的位置，元素的宽高和其他属性不会失效

3. opacity: 0

元素的透明度为0，同样起到隐藏元素的作用，但元素隐藏之后，会占据着原来的位置， 宽高都不会失效
```



##### 9. 清除浮动的方式

```css
1. 额外标签法(在最后一个浮动标签后面，新加一个标签，给其设置clear：both)

缺点: 添加无意义的标签，语义化较差

<div class="clear"></div>

.clear{
    clear:  both
}


2. 让父元素触发BFC

可以给父元素设置  
overflow: hidden   
display: inline-block
float: left/right
position: absolute 、 fixed

缺点: 添加这些属性时，就要为这些属性带来的副作用买单


3. 利用after伪元素清除浮动， 在父元素上加上伪元素(推荐使用这种方法)

这种方式需要注意，必须要给伪元素设置成块级元素
.wrap:after{
    content: '',
    display: block,
    clear: both
}
```



##### 10 什么是重绘与重排

```
回答：可以从什么是重绘与重排， 引起重绘重排的原因， 哪些属性会导致重绘与重排这几方面来回答

1. 什么叫重排与重绘

重排：Dom的变化会让浏览器重新计算元素的几何属性，其他元素的几何属性也会受到影响，浏览器需要重新构造渲染树，这个过程被称为重排。
重绘：浏览器将受到影响的部分重新绘制在屏幕上的过程称为重绘 

2. 引起重绘与重排的原因

添加或者删除可见的Dom元素
元素位置或者尺寸的改变
浏览器页面初始化
浏览器窗口大小发生改变
重排一定导致重绘， 重绘不一定导致重排

3. 哪些属性的改变会导致重绘与重排

1. 重排

width,  height,  padding,  margin,  display,  border-width,  border,   top,  left,  right,  bottom,  position,   font-size,   text-align,   font-weight, overflow,   font-family


2. 重绘

color,   border-style,    background,    background-image,   visibility
box-shadow,     border-radius,    outline,    outline-color,   background-size
```



##### 11 怎么用css画三角形

```css
div{
    width: 0;
    height: 0;
    border: 100px solid red;
    border-top-color: transparent;   // 颜色设为透明
    border-left-color: transparent;
    border-right-color: transparent;
}

canvas方式

先要确定你要画什么样的三角形，然后确定三个点，通过moveTo, lineTo,把三个点连接起来， 然后再通过绘制方法如果是空心的就通过stroke，如果是实心的就通过fill方法
```



##### 12 响应式布局的常用解决方案有哪些

```js
1. 什么是响应式布局？

响应式布局就是一种通过css，可以实现页面的自适应调整，可以使页面在不同设备，或者在不同的屏幕尺寸上有良好的展示效果


2. 有哪几种解决方案

 2.1. 设置百分比单位

 2.2. rem布局
     rem是相对于根元素的字体大小来计算的，如果根元素的字体大小为16px，那么1rem = 16px
     
     em是相对于父元素的字体大小来计算的，如果父元素的字体大小为16px，那么给子元素设置为1em = 16px
     
     rem布局有自己的缺陷，它是必须通过js来动态的控制根元素的font-size的大小，也就是说css样式和js代码有一定的耦合性

 2.3. vh和vw, css3新引进的单位，它与视图窗口有关， vh和vw就是根据窗口的宽高分成100等份
 
 2.4. 媒体查询

     @media媒体查询是针对不同的媒体类型定义不同的样式，特别是响应式页面，可以针对不同屏幕的大小，编写多套样式，从而达到自适应效果
     但缺点也很明显，当样式需要改变很多时，就会有好多套样式代码，会显得很复杂
```

