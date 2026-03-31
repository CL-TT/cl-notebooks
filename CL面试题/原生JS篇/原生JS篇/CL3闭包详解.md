# 闭包详解

## 1.什么是闭包

1. 通俗的来讲  **可以访问函数内部变量的函数叫做闭包**

####   2.实际上是这样的一个过程

​     2.1. 一个函数里面有一个子函数， 子函数是可以访问这个父函数的局部变量

​     2.2. 通过return这个子函数将他暴露在全局作用域内，按道理讲在外部是访问不到那个局部变量的， 但这个子函数形成了闭包

​     2.3. 通过闭包，父函数的局部变量没有被销毁， 就可以通过闭包在外部访问这个局部变量



```javascript
function a(){
    var name = 'cl';
    
    function b(){
        console.log(name);
    }
    
    return b;
}

var c = a();

c();   //会打印出cl

//原理是这样的
//1. a函数被定义时， 他的作用域 scope 索引0 是指向全局作用域的 也就是GO
//2. a函数在执行的时候， 会形成自己的执行期上下文也就是AO，把它叫做aAO, 这个时候它的作用域发生了变化
//   索引0指向了自己的AO， 索引1指向了GO
//3. b函数在被定义时，他享受了a函数的劳动成果， 所以b函数的scope应该是 索引0指向aAO,  索引1指向GO
//4. a函数执行完之后，会销毁自己的AO。
//5. b函数在外部被执行时，会形成自己的执行期上下文， 形成自己的AO， 把它叫做bAO
//   他的作用域又发生了变化， 0 => bAO   1 =>  aAO  2  =>  GO
//6. a函数的AO被销毁了，你b函数在外面凭什么还能访问到我的变量， 这时候b函数得意的说，你销毁了没用，我手里还保留了一份你a函数的AO呢， 所以在外部是可以访问到a函数的局部变量的， 这也就是闭包的整个过程。 
```



## 2.闭包的优缺点

### 2.1缺点

```js
1. 闭包长期占用内存， 内存消耗很大， 可能导致内存泄漏

什么是内存泄漏？

浏览器自身有一套内存回收机制， 当分配出去的内存不使用的时候便会回收， 内存泄漏的根本原因就是你的代码中分配了一些顽固的内存，浏览器无法进行回收， 如果这些顽固的内存还在一直不停的分配就会导致后面的内存不足，造成泄漏
```



### 2.2优点

```javascript
//1. 实现公有变量， 函数累加器
//在没有闭包这种情况下， 要实现累加器的话， 只有在外部定义一个全局变量， 然后在函数内对这个变量就行操作


function add(){
    var count = 0;
    
    return function(){
        ++count;
        console.log(count);
    }
}

var myAdd = add();

myAdd();
```

```javascript
//2. 可以做缓存， 存储结构像缓存


function test(){
    var food = 'apple';
    
    return {
        eat: function(){
            if(food){
                console.log('i am eating' + food);
            }else{
                console.log('nothing');
            }
        },
        
        push: function(newFood){
            food = newFood;
        }
    }
}

//缓存的结构就是， 你往一个里面存储一个值时， 下次取的时候永远都是新存入的值
```

```javascript
//3. 实现属性私有化

function Person(friends){
    var name = 'caolei';
    
    this.friends = friends;
    
    this.getFriends = function(){
        this.friends = name;
    }
}

var prople = new Person('haha')

Person.getFriends();

//这样在外部执行的getFriends()这个函数和Peoson这个构造函数形成了闭包,  外部的people却访问不到这个变量
```

