# NodeJs的生命周期

## 1.过程

![eventloop1](C:\Users\caolei\Pictures\笔记图片\eventloop1.png)

1. 进入主线程到程序入口
2. 进入事件循环
3. 看事件循环还有没有工作，如果没有那么程序结束



## 2.事件循环这个过程

![eventloop2](C:\Users\caolei\Pictures\笔记图片\eventloop2.png)

```javascript
//1. Timers  主要是定时器和计时器相关的回调函数

//2. Poll   轮询队列   大部分回调都会放入该队列，比如文件的读取，用户的监听等
//Poll的运作方式  
//如果poll中有回调，依次执行回调， 直到清空队列
//如果poll中没有回调，等待其他队列出现回调，结束该阶段，进入下一阶段。  如果其他阶段也没有回调持续等待直到其他队列出现回调为止。

//3. Check检查阶段   如果出现setImmediate()直接进入这个阶段
```



## 3.微任务 nextTick和promise

1. nextTick和promise他们不属于事件循环的一部分。
2. 事件循环中每次打算执行一个回调之前，都会清空nextTick和promise队列。



## 4.实例解析

### 1.实例1

```js
setImmediate(() => {
  console.log(1);
})

process.nextTick(() => {
  console.log(2);

  process.nextTick(() => {
    console.log(3);
  })
})

console.log(4);

Promise.resolve().then(() => {
  console.log(5);

  process.nextTick(() => {
    console.log(6);
  })
})


//这段程序的执行过程

1. 先执行同步代码   会先输出4

2. 有一个setImmediate会把它的回调函数放到check队列中，等待执行

3. 在把nextTick的回调函数放到, 微任务的nextTick队列中

4. 在把promise的回调函数放到微任务的promise的队列中

5. 走时间循环， timer队列中没有回调函数， poll轮询队列中也没有回调函数

6. 当要执行check队列中的回调函数时， 会先执行nextTick和promise，  所以会输出2， 再输出3

7. 执行promise的回调函数会输出5， 再输出6

8. 然后执行check队列中的回调函数，输出1

9. 总的顺序就是  4 2 3 5 6 1
```



### 实例2

```js
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}

async function async2() {
  console.log("async2");
}

console.log("script start");

setTimeout(function () {
  console.log("setTimeout0");
}, 0);

setTimeout(function () {
  console.log("setTimeout3");
}, 3);

setImmediate(() => console.log("setImmediate"));

process.nextTick(() => console.log("nextTick"));

async1();

new Promise(function (resolve) {
  console.log("promise1");
  resolve();
  console.log("promise2");
}).then(function() {
  console.log("promise3");
});

console.log("script end");


/**
 * 执行顺序解析
 * 1. 先执行同步代码, 会先输出 'script start'
 * 
 * 2. 遇到setTimeout 放入到timer队列中  'setTimeout0'
 * 
 * 3. 遇到setTimeout 放入到timer队列中  'setTimeout3'
 * 
 * 4. 遇到setImmediate 放入到check队列中 'setImmediate'
 * 
 * 5. 遇到nextTick, 放入到微任务的nextTick队列中  'nextTick'
 * 
 * 6. 这个时候执行了async1()函数， 先输出  'async1 start'
 * 
 * 7. 然后执行async2(),  输出 'async2', 因为是async函数返回的是promise放入到promise队列中 'async1 end'
 * 
 * 8. 遇到promise, 先输出同步代码 'promise1' 'promise2'  然后then函数放入到微任务的promise对列中 'promise3'  
 * 
 * 9. 输出同步代码  'script end'
 * 
 * 10. 进入事件循环  event loop  , 想执行事件循环中的队列中的函数，就必须要清空微任务中的nextTick
 *     和promise队列中的回调函数, nextTick优先, 所以会先输出  'nextTick',  'async1 end'  'promise3'
 * 
 * 11. 如果前面这些代码运行时间已经过了3毫秒，  那么会先输出 'setTimeout0'  'setTimeout3'
 * 
 * 12. 最后在执行check队列中的回调函数  'setImmediate'
 * 
 * 13. 'script start' => 'async1 start' => 'async2' => 'promise1' => 'promise2'
 *     => 'script end' => 'nextTick' => 'async1 end' => 'promise3' => 'setTimeout0' 
 *     => 'setTimeout3' => 'setImmediate'
 */
```

