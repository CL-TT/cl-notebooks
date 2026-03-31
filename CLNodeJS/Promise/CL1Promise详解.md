# Promise详解

## 1. Promise简介

Promise是解决异步编程的一种方法，是缓和回调地狱的一种手段，其实本身是一个构造函数。



## 2. Promise的三种状态详解

1. pending 等待状态

2. fulfilled / resolved 成功状态

3. rejected 失败状态

   对象的状态是不受外界影响的，只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。

### 2.1 自己理解的promise

​        promise是承诺的意思，我认为在promise里面的异步操作给promise对象一个承诺，如果我这个异步操作成功了那么我就完成了我给你的承诺，那么promise对象就会把自己的状态pending等待状态变成fulfilled成功状态，如果失败了那么就没有完成承诺，那么promise对象就会把自己的状态pending等待状态变成rejected失败状态。



## 3. Promise的基本使用

```javascript
var promise = new Promise(function(resolve, reject){
    //这个是异步操作
    fs.readFile(path, function(error, data){
        if(error){
            reject(error);    //如果这个异步操作失败了，那么可以调用reject
        }else{
            resolve(data);   //如果这个异步操作成功了， 那么可以调用resolve
        }
    })
})

//这个promise的妙处就在于把这个回调分离了，放在then函数里面执行了

promise.then(function(data){
    console.log(data);
}, function(error){
    console.log(error);
})

//1.then函数里面的第一个参数代表着 resolve函数
//2.then函数里面的第二个参数代表着 reject函数
```

