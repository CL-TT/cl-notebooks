# 小程序问题

##### 1. 在点击input输入框时， 怎么阻止手机自带的键盘？



##### 2.在用vant组件库的uploader组件时，在加上 max-duration属性时在真机调试时点击组件出现不了选择图片或者拍摄视频的弹出框。



##### 3.页面跳转传参时，如果参数过长，可以先编码，然后在解码接收。

 3.1   编码  const temp = encodeURIComponent(JSON.stringify(detailObj));

 3.2   解码  const temp = JSON.parse(decodeURIComponent(data));



##### 4.在完成自定义组件tabbar，在切换tabbar时，会出现样式不对等的情况，有一种异步的感觉，在使用vant组件库的tabbar组件同样有这种问题。

  解决方案：

1. 在app.js中  globalData中定义一个变量  selectedIndex: 0
2. 在切换tabbar的方法中通过app.globalData.selectedIndex = ???    来改变这个值
3. 在组件的生命周期函数ready中设置，  selected: app.globalData.selectedIndex



##### 5.vant组件库的日历组件中的formatter怎么动态的自定义日期文案。

```javascript
data: {
    formatter: function(day){
        ...
        //这个formatter函数是在渲染的时候就会执行， day参数是组件自己传过来的，并且this是指向那个日期对象的
    }
},
    
在某些方法中可以通过重新设置formatter函数
this.setData({
    formatter: function(day){
        //可以在这里获取动态请求过来的数据
        //这样设置this还是指向日期对象的
    }
})
```

