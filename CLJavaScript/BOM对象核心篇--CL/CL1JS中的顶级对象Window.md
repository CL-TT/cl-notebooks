# JS中的顶级对象Window

## 1. Window对象上的属性

#####   1. innerWidth: 获取浏览器窗口的宽度，不包含工具条和滚动条的宽度

#####   2. innerHeight: 获取浏览器窗口的高度， 应用场景一般用于获取浏览器的定高和定宽



#####   3. pageXOffset:  设置或返回当前页面相对于窗口显示区左上角的 X 位置,也就是记录滚动条横向滚动的距离

#####   4. pageYOffset: 设置或返回当前页面相对于窗口显示区左上角的 Y 位置,也就是记录滚动条纵向滚动的距离

   ***这两个属性一般配合着懒加载预加载的功能一起使用***



#####   5. scrrenX:  只读属性, 声明了浏览器窗口的左上角在整个电脑屏幕上的的 x 坐标

#####   6. scrrenY:  声明了浏览器窗口的左上角在整个电脑屏幕上的的 y 坐标

######         IE、Safari、Chrome 和 Opera 支持 screenLeft 和 screenTop

######         Chrome、Firefox 和 Safari  支持 screenX 和 screenY

```javascript
兼容性写法

const width = window.scrrenX || window.scrrenLeft
```





#####   7. name:   给浏览器起一个名称，在跨域访问的时候有作用





## 2. Window上的方法

####    1. window.open()    打开一个新的浏览器窗口

```javascript
window.open('地址名称', '浏览器窗口名称', '浏览器窗口的特征值')
```



####   2. window.close()    关闭浏览器窗口

```javascript
window.close()
```



####  3. window.prompt()   显示可提示用户输入的对话框

```javascript
window.prompt('message')
```



####  4. window.confirm()   显示带有一段消息以及确认按钮和取消按钮的对话框,返回值是布尔值

```javascript
window.confirm('message')
```

