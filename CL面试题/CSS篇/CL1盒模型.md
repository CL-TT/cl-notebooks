# 盒模型

## 1.什么是盒模型

CSS中所有的HTML元素都可以看成是一个盒子，一个盒模型包括 外边距margin  边框border  内边距padding  内容content这四个部分。



## 2.盒模型分为几种

盒模型分为两种第一种叫做标准盒模型，第二种叫做怪异(也叫IE)盒模型



## 3.标准盒模型和怪异盒模型的区别

1. 标准盒模型  

   给盒子设置width时:    width = content区域的宽高

   盒子真正的宽高：width + padding + border + margin

2. 怪异盒模型

   给盒子设置width时： width =  content + padding + border区域的宽高 

   盒子的真正宽高：width + margin

## 4.css中那个属性决定使用什么盒模型

```css
//box-sizing 属性
1.  :content-box     默认值也就是标准盒模型
2.  :border-box      怪异盒模型
3.  :inherit         继承父元素的box-sizing值
```



## 5.怪异盒模型的应用场景

​     1.当宽度不固定，内边距固定时

​     2.一个元素宽度固定，他的子元素宽度100%， 却超出了父元素的内容区

​    