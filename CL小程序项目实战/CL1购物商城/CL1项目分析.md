# 项目分析

## 1. 项目搭建

## 2. 搭建目录结构

| 目录名     | 作用         |
| ---------- | ------------ |
| styles     | 存放公共样式 |
| components | 存放组件     |
| lib        | 存放第三方库 |
| utils      | 工具库       |
| request    | 接口库       |
| icons      | 放置图标     |



## 3. 搭建项目页面

| 页面名称     | 名称         |
| ------------ | ------------ |
| 首页         | index        |
| 分类页面     | category     |
| 商品列表页面 | goods_list   |
| 商品详情页面 | goods_detail |
| 购物车页面   | cart         |
| 收藏页面     | collect      |
| 订单页面     | order        |
| 搜索页面     | search       |
| 个人中心页面 | user         |
| 意见反馈页面 | feedback     |
| 登录页面     | login        |
| 授权页面     | auth         |
| 结算页面     | pay          |



## 4. 引入字体图标

1. 借助阿里巴巴的图标库即可
2. 把图标加入到项目中，然后点击font class
3. 点击在线链接，把代码copy到style文件夹下的iconfont.wxss中
4. 在全局的样式文件中引入使用即可



## 5. 搭建tabbar

```json
//1. list这个数组长度至少要大于等于2

"tabbar": {
    "list": [
        {
            "pagePath": "",
            "text": "",
            "iconPath": "",
            "selectedIconPath": ""
        }
    ]
}
```



## 6. 初始化页面样式

1. 在app.wxss中进行一些样式的初始化
2. 元素的margin, padding,初始化
3. 定好主题色， 定好字体大小