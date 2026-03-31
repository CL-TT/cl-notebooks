# CSS3之Border边框属性

### 1. border-radius 元素圆角属性

1. 元素的圆角属性， 最大的范围是根据元素的宽高而言的， 能调的范围是元素的最大宽度和最大高度， 超过了这个值是没有效果的。

2. border-radius这个属性的最终形态或者说设置一个角的方式为  border-top-left-radius: 10px 10px

   第一个参数是逐渐向x轴压的， 第二个参数是逐渐向y轴压的，  实际上第一个参数是圆的横向半径，第二个参数是圆的纵向半径。然后按照这个圆来切割元素，元素的圆角就是这么来的

   ![](C:\Users\caolei\Pictures\CSS3\border-radius.png)



### 2. box-shadow 边框阴影属性

1. 元素的边框阴影
2. 0px(横轴的偏移量)    0px(纵轴的偏移量)    0px(模糊值)    0px(扩展值)    color(颜色值)
3. 模糊值，是根据自身的那条边向两边同时展开模糊
4. inset 内阴影
5. 阴影在文字的下面， 在背景颜色的上面
6. 如果有多层阴影，第一个写的阴影在最上面，以此类推



### 3. border-image 边框使用图片来填充

1. border-image-source 图像来源路径 {

     url()

​         linear-gradient(yellow, red)

   }

2. border-image-slice 边框背景的分割方式，以九宫格为规则，以数字为单位进行分割， 必须要写的
3. border-image-width  边框的宽度
4. border-image-outset 边框背景图的扩展
5. border-image-repeat 边框图像的平铺方式 {

​        stretch: 拉伸   repeat: 重复铺满   round: 重复并调整

   }