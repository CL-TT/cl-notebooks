# 关键字async和await

## 1.async的作用

1. 目的是简化在函数的返回值中对Promise的创建
2. 被async修饰的函数字面量或者表达式，他的返回值最终都将返回一个Promise对象

```javascript
async function a(){
    console.log('haha');
    return 12;
}

//上面的代码就相当于
function a(){
    return new Promise((resolve, reject) => {
        consoel.log('haha');
        resolve(12);
    })
}
```



## 2.await的作用

**await关键字必须出现在async函数中！！！！**

1. await用在某个表达式之前，如果表达式是一个Promise，则得到的是thenable中的状态数据。

多为resolve状态的数据， 如果想得到rejected状态的数据，就要捕获错误

```javascript
async function a(){
    return 1;
}

async function b(){
    let data = await a();   //一定会等出结果了才会执行下面的代码
    console.log(data);   //会打印1
}

//上面的代码相当于
function b(){
    return new Promise((resolve, reject) => {
        a().then(data => {
            let result = data
            console.log(result);
            resolve();
        })
    })
}


```



2. 如果await的表达式不是Promise，则会将其使用Promise.resolve包装后按照规则运行