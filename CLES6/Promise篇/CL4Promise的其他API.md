# Promise的API总结

## 1.原型成员(实例成员)

- then：注册一个后续处理函数，当Promise为resolved状态时运行该函数

- catch：注册一个后续处理函数，当Promise为rejected状态时运行该函数

- finally：[ES2018]注册一个后续处理函数（无参），当Promise为已决时运行该函数

  ```javascript
  const promise = new Promise((resolve, reject) => {
      resolve(1);
  })
  
  //不管什么状态都会执行这个finally函数， 这个函数是没有参数的
  promise.finally(() => {
      console.log('haha')
  })
  ```

## 2.构造方法(静态成员)

- resolve(数据)：该方法返回一个resolved状态的Promise，传递的数据作为状态数据

  - 特殊情况：如果传递的数据是Promise，则直接返回传递的Promise对像

    ```javascript
    const promise = new Promise((resolve, rejct) => {
        resolve(1)
    })
    
    //相当于
    const promise = Promise.resolve(1);
    ```

    

- reject(数据)：该方法返回一个rejected状态的Promise，传递的数据作为状态数据

- all(iterable)：这个方法返回一个新的promise对象，该promise对象在可重复的参数对象里所有的promise对象都成功的时候才会触发成功，一旦有任何一个可重复的参数里面的promise对象失败则立即触发该promise对象的失败。这个新的promise对象在触发成功状态以后，会把一个包含iterable里所有promise返回值的数组作为成功回调的返回值，顺序跟iterable的顺序保持一致；如果这个新的promise对象触发了失败状态，它会把iterable里第一个触发失败的promise对象的错误信息作为它的失败错误信息。Promise.all方法常被用于处理多个promise对象的状态集合。

  ```javascript
  //Promise.all(arr) 多用于处理这个全部达到什么状态， 再执行什么任务， 
  //所有的promise对象都成功的时候才会触发成功，   会把返回值放到一个数组中
  //只要有一个失败了，就会触发失败
  ```

  

- race(竞赛)：当iterable参数里的任意一个子promise被成功或失败后，父promise马上也会用子promise的成功返回值或失败详情作为参数调用父promise绑定的相应句柄，并返回该promise对象



```javascript
//1.在多个promise对象中一旦发现子promise成功或失败， 就立马停止，可以输出成功的返回值，或失败详情

const promise = Promise.race([]).then(data => {
    
}, err => {
    
})
```

