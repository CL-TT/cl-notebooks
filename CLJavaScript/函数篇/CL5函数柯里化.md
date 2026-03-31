# 函数柯里化

## 1. 含义以及作用

#### 函数柯里化的基本含义

1. 函数柯里化是把接收多个参数的函数变换成接受一个单一参数(最初函数的第一个参数)的函数，并且返回接受余下的参数的新函数的技术（光看含义很难理解这个柯里化，所以我们接着往下看）
2. 我们来看一个例子，最简单的求和函数

```javascript
function add(x, y){
    return x + y;
}

add(1, 2)     //得到的结果是3

//用柯里化来实现
function curAdd(x){
    return function(y){
        return x + y;
    }
}

curAdd(1)(2);   //得到的结果还是3

就是把add函数的参数进行拆分传递， 先传第一个参数，得到的是一个函数让这个函数去接受剩余的参数， 再传第二个参数， 然后得到结果
```



#### 柯里化的实际作用

1. 为什么要使用柯里化呀，他能够带来哪些好处呀
2. 柯里化的最大的作用就是参数复用(把参数固定下来)（利用的就是js中的预处理的思想，降低通用性，提高适用性），先来看一个例子

```js
//比如说现在有一个ajax函数
function ajax(method, url, data){}

//我要使用这个ajax函数
ajax('post', 'www.caolei.com', {name: 'caolei'});
ajax('post', 'www.ceokk.com', {name: 'clei'});

//我们可以发现，上面这两个函数的调用，明明第一个参数都是相同的都是post方法，但是每一次调用还是传了相同的参数，这样写起来很不爽

//如果用柯里化的方式来写呢， 这样第一个参数就复用了
const postAjax = curAjax('post');

postAjax('www.caolei.com', { name: 'caolei' });
postAjax('www.ceokk.com', { name })
```



## 2. 封装一个通用的方法

```js
/**
* fn: 给我传一个处理函数
* args1: 剩余参数，可以传参数，也可以不传
**/
function curry(fn, ...args1){
    //先获取要处理的函数的形参个数
    const fnParamsLen = fn.length;
    
    return function(...args2){
        const allArgs = [...args1, ...args2];
        
        //如果你传的参数已经达到要求了, 那么直接调用处理方法就行了
        if(allArgs.length >= fnParamsLen){
            return fn.apply(this, allArgs);
        }else{
            //如果还没有达到参数个数要求， 先把传入的参数保存起来，递归调用curry方法, 这个递归的出口就是直到你传的参数，达到了处理函数参数的要求
            return curry(fn, ...allArgs)
        }
    }
}
```



## 3. 用封装的方法来做这个面试题

```js
// 实现一个add方法，使计算结果能够满足如下预期：
add(1)(2)(3)(4) = 10;
add(1, 2, 3)(4) = 10;
add(1)(2)(3, 4) = 10;

function newAdd(n1, n2, n3, n4){
    return n1 + n2 + n3 + n4;
}

const add = curry(newAdd);
```

