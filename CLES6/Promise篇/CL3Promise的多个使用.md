# Promise的多个使用

1. 当后续的Promise需要用到之前的Promise的处理结果时，需要Promise的串联

Promise对象中，无论是then方法还是catch方法，它们都具有返回值，返回的是一个全新的Promise对象，它的状态满足下面的规则：

1. 如果当前的Promise是未决的，得到的新的Promise是挂起状态

2. 如果当前的Promise是已决的，会运行响应的后续处理函数，并将后续处理函数的结果（返回值）作为resolved状态数据，应用到新的Promise中；如果后续处理函数发生错误，则把返回值作为rejected状态数据，应用到新的Promise中。

   ```javascript
   //1.后续的promise能不能用 -> 取决于前面那个promise处于哪个阶段
   //  如果是未决那么新的promise就挂起， 如果是已决那么新的promise就可以应用
   
   //2.后续的promise是应用哪一个状态函数 -> 取决于前面promise的处理函数有没有报错
   //  如果没有报错，那么就把返回值作为resolved状态数据，  如果报错，那么就把返回值作为rejected状态数据
   ```

   

**后续的Promise一定会等到前面的Promise有了后续处理结果后，才会变成已决状态**

2. 如果返回的是promise对象

   如果前面的Promise的后续处理，返回的是一个Promise，则返回的新的Promise状态和后续处理返回的Promise状态保持一致。

```javascript
const promise = new Promise((resolve, reject) => {
    resolve(2);
})

const promise2 = promise.then(data => {
    //返回的是一个新的promise对象
    return new Promise((resolve, reject) => {
        resolve(data);
    })
})

promise2.then(data => {
    console.log(data);
})

//1. promise2的状态取决于， return的那个promise的状态
//2. promise2到底指向哪个状态取决于，return的那个promise有没有报错
//3. 数据不管是date还是error, 取决于上面的data和error
```

