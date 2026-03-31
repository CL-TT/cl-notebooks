# 几种移动端的适配方式

## 1. 百分比适配

1. 他需要知道父级元素的宽度



## 2. ViewPort缩放适配

1. ViewPort是**用户网页的可视区域**，被称为视区

2. ViewPort的属性

   #1. width: 控制viewport的大小，可以指定一个值如600，或者特殊的值，如device-width为设备的宽度

   #2. height: 和width相对应，指高度

   #3. initial-scale: 初始缩放比例，也就是当页面第一次load的时候缩放比例

   #4. maximum-scale: 允许用户缩放到的最大比例

   #5. minimum-scale: 允许用户缩放的最小比例

   #6. user-scalable: 用户是否可以手动缩放

3. viewport缩放适配的核心内容

   #1. 理念：把所有机型的设备独立像素设置成一致

   #2. 比例公式  **缩放比 = css像素 / 目标像素（你想要设置成的宽度）**

4. 缺点: 这样设置一个目标宽度，会使所有手机的宽度都被固定为一个值，那么大屏手机就没有意义了

```js
(function(){
    //1.先要获取到每个不同设备的宽度
    const screenWidth = document.documentElement.clientWidth;
    console.log('宽度', screenWidth);

    const screenWidth2 = window.screen.width;
    console.log('宽度2', screenWidth2);

    const screenWidth3 = window.innerWidth;
    console.log('宽度3', screenWidth3);

    //2. 设置目标像素
    const target = 375;

    //3. 计算缩放比
    const scalePoint = screenWidth / target;

    //4. 取出meta标签
    const meta = document.querySelector('#meta');

    console.log('meta', meta);
    console.log(meta.content);
    //5. 设置meta标签的content属性值，来控制缩放比
    meta.content = `initial-scale=${scalePoint} , minimum-scale=${scalePoint} , maximum-scale=${scalePoint}`
})()
```





## 3. DPR配合rem的适配方式（最佳实践）

1. rem是相对于根元素的字体大小而言的 
2. em是相对于父元素的字体大小而言的

```js
/**
 * document
 * window
 * dec:设计稿的宽度
 * 只要知道元素在设计稿里面的宽度,然后除以100,加上单位rem就行了
 **/
 (function(doc, win, desWidth){
     const html = doc.documentElement;
     const refRefshRem = () => {
         const htmlWidth = html.clientWidth;

         if(htmlWidth > desWidth){
            //如果设备的宽度要远远大于设计稿的宽度, 那么就定死font-size的值
            html.style.fontSize = '100px';
          }else{
            //html.style.fontSize = 16 * htmlWidth / 375 + 'px';   //这个是以iPhone6作为一个基准


            // 375 / 750 = 0.5
            html.style.fontSize = 100 * (htmlWidth / desWidth) + 'px';  //这个式子已经除了DPR的值
        }
      };

      doc.addEventListener('DOMContentLoaded', refRefshRem);   //dom一加载完就执行这个函数
})(document, window, 750)
```





## 4. vw和vh适配

1. vw相当于直接把屏幕分成了100份，1vw就是1列

```js
那么按照i6/7/8设备宽度为375px,  那么1vw就是3.75px

html.style.fontSize = 100 * (clientWidth / designWidth) + 'px'
= 100 * (375 / 750(这就是设计稿的宽度))
= 100 * 0.5
= 50px

那么就要计算出50px对应几个vw,  50 / 3.75 = 13.33333333vw

:root{
    font-size: 13.33333333vw
}

```

