# 添加事件的几种方式

## 1. 原生方法 elem.on + 事件名称

```javascript
//1. elem.on + 事件名称 = function(){};

//2. 这个只能绑定一个处理函数， 如果绑定多了，那么就执行最后一个绑定函数

//3. this指向调用这个方法的元素本身

const div = document.querySelector('div');

div.onclick = function(){
    console.log('a');
}

div.onclick = function(){
    console.log('b');
}
```



## 2. 原生方法 elem.addEventListener

```javascript
//1. elem.addEventListener('事件类型', 处理函数, 布尔值=>是否发生捕获);
//捕获是自父元素向子元素的， 点击父元素会触发子元素的事件

//2. 这个可以绑定多个处理函数，绑定多个在执行时都会触发

//3. this还是指向调用这个方法的元素本身

const div = document.querySelector('div');

div.addEventListener('click', function(){
    console.log('a');
}, false);

div.addEventListener('click', function(){
    console.log('b');
}, false)


```



## 3. IE独享 elem.attachEvent

```javascript
//1. elem.attachEvent('on事件类型'， function(){})

//2. 他同样可以绑定多个处理函数

//3. this指向window

div.attachEvent('onclick', function(){
    console.log('a');
})
```

