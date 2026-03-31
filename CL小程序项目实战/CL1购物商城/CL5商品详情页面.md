# 商品详情页面

## 1. 详情商品的轮播图展示(swiper组件)

1. 先从云数据库中获取所需要的动态数据
2. 然后把数据动态渲染到轮播图中

![detail-swiper](C:\Users\caolei\Pictures\小程序分析\购物商城\detail-swiper.png)

## 2. 商品的文字介绍

1. 商品的价格渲染

2. 商品的名称，以及收藏icon

3. 点击收藏按钮，会触发收藏功能事件

   3.1 思路还是利用缓存进行处理

   3.2 点击时判断是哪种状态

   3.3 如果是收藏，那么就取消收藏， 并把商品从收藏数组中移除，并重新设置缓存

   3.4 如果不是收藏， 那么就收藏， 并把商品加入到收藏数组，并重新设置缓存

![detail-text](C:\Users\caolei\Pictures\小程序分析\购物商城\detail-text.png)

## 3. 商品的图文详情

1. 这个是用HTML字符串进行展示的

2. 这里用到了富文本rich-text组件中的nodes属性来展示HTML字符串

   <rich-text nodes="html字符串" ></rich-text>

![detail](C:\Users\caolei\Pictures\小程序分析\购物商城\detail.png)



## 4. 底部工具栏

1. 客服回话功能， 点击没有效果，   <button open-type="concact"></button>
2. 分享功能   open-type="share"
3. 购物车，点击过后进入购物车页面，这里要注意如果是通过navigator跳转的话， 因为购物车页面是tabbar页面， 这样是跳转不过去的， 所以需要加上open-type="switchTab"这个属性
4. 加入购物车功能，  把数据保存到缓存中。

![detail-util](C:\Users\caolei\Pictures\小程序分析\购物商城\detail-util.png)