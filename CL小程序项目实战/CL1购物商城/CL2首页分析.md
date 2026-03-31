# 首页分析

## 1. 搜索框组件(自定义组件)

1. 点击搜索框组件会进行跳转， 跳转到搜索页面

2. 跳转页面的路径我不想写死， 想写成动态数据

   ![search](C:\Users\caolei\Pictures\小程序分析\购物商城\search.png)



## 2. 轮播图部分(swiper组件)

1. 从接口获取轮播图数据， 发送异步请求来获取数据， 这里我是用云数据库来保存数据的

2. 根据这些数据来完成轮播图

   ![swiper](C:\Users\caolei\Pictures\小程序分析\购物商城\swiper.png)



## 3. 分类导航

1. 先从云数据库中获取到数据

2. 根据这些数据来动态渲染出结构

   ![category](C:\Users\caolei\Pictures\小程序分析\购物商城\category.png)



## 4. 楼层结构

1. 从云数据库中获取数据

2. 然后动态渲染， 注意样式

   ![floor](C:\Users\caolei\Pictures\小程序分析\购物商城\floor.png)