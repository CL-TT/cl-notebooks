# 生成器详解

## 1.生成器的基本概念

1. 生成器是一个通过构造函数Generator创建的对象，**生成器既是一个迭代器，同时又是一个可迭代的对象**（所以这个对象中有next方法，具有具名符号Symbol.iterator）, 但是不能通过构造函数创建，因为这个只是供js内部创建的
2. 迭代器难在怎么去写一个迭代器，而生成器的问世就是为了简化迭代器的写法



## 2.生成器的创建与使用

```js
//生成器的创建必须使用生成器函数,  该函数一定返回一个生成器

function *generator(){
    
}

1. 运行该生成器函数，并不执行函数里面的代码，只是生成一个生成器而已

2. 生成器函数内部是为了给生成器的每次迭代提供数据的

3. 每次调用生成器的next方法，将导致生成器函数运行到下一个yield关键字位置就停止

4. yield是一个关键字，该关键字只能在生成器函数中使用，表示产生一个迭代数据 { value: '', done: '' }


function *createGenerator(){
    console.log('第一次');
    yield 1;
    
    console.log('第二次');
    yield 2;
    
    console.log('第三次');
    yield 3;
}

const generator = createGenerator();  //生成一个生成器，此时并没有运行函数

const data = generator.next();   
//调用next方法就运行了一次生成器函数，  此时控制台会打印“第一次”，并且data为{ value: 1, done: false }, false指还有数据，true表示接下来已经没有数据了

generator.next();  //再次执行，控制台打印“第二次”，并且data为{ value: 2, done: false }

generator.next();  //再次执行，控制台打印“第三次”，并且data为{ value: 3, done: true }

generator.next();  //再次执行，data为{ value: undefined, done: true }
```



## 3.生成器细节之处

1. 生成器函数可以有返回值，返回值出现在第一次done为true时的value属性中，之后再调用next, value值还是undefined

```js
function *createGenerator(){
    console.log('第一次');
    yield: 1;
    
    return 10;
}

const generator = createGenerator();

generator.next();  //{ value: 1, done: false }

generator.next();  //{ value: 10, done: true }

generator.next();  //{ value: undefined, done: true }
```

2. 调用生成器的next的方法，可以传递参数，**传递的参数会交给yield表达式的返回值，一定要有表达式才能接收到参数**，第一次调用next方法时，传参没有任何意义

```js
function *createGenerator(){
    console.log('第一次');
    const info = yield 1;
    
    consoel.log(info);
    console.log('第二次');
    yield 2 + info;
}

const generator = createGenerator();

generator.next();   //控制台会打印“第一次”， { value: 1, done: false }

generator.next(3);  //控制台会输出3， 会打印”第二次“，{ value: 5, done: false }

generator.next();  // { value: undefined, done: true }
```

3. 在生成器函数内部，可以调用其他生成器函数，但是再调用的时候要加上*号

```js
function *test1(){
    yield 1;
    yield 2;
}

function *test2(){
    yield *test1()
    
    yield 3;
}

const t = test2();

t.next();   // { value: 1, done: false }
```



## 4.生成器的其他API

1. return： 调用该方法，可以提前结束生成器函数，从而提前让整个迭代过程结束
2. throw：调用该方法，可以在生成器中产生一个错误

```js
function *test(){
    yield 1;
}

const t = test();

t.return()   { value: undefined, done: true }
```



## 5.生成器应用之异步任务控制

```js
//在还没有async await关键字的这段空档期，为了更方便的使用promise这种处理异步问题的方式， 所以有的人用生成器来进行异步任务的控制

//利用生成器来模仿async await关键字

function *generatorTask(){
  const a = yield 1;
    
  const res = yield fetch('');
    
  const str = yield 'str';
}

function run(task){
    //1. 先生成一个生成器
    const generator = task();
    
    //2. 在启动生成器
    let result = generator.next();
    
    handleResult(result);
    
    //3. 一个处理结果的函数
    function handleResult(result){
        if(result.done) return; //后面没有值了，不作处理
        
        //判断是正常逻辑还是promise
        if(typeof result.value.then == 'function'){
            //是promise的状况
            result.value.then(res => {
                result = generator.next(res);
                handleResult(result);
            }, error => {
                result = generator.throw(error);
                handleResult(result);
            })
        }else{
            //正常逻辑
            result = generator.next(result.value);
            handleResult(result);
        }
    }
}

run(generatorTask);
```

