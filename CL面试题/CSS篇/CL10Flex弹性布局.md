# Flex弹性布局

## 1. flex里面的属性

```javascript
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



## 2. flex:1是有哪几种属性组成的

flex: 1是由flex-grow,  flex-shrink,   flex-basis这三个属性组成的