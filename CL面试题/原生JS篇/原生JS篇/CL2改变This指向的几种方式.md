# 改变This指向的几种方式

## 1.call和apply方法

call和apply不同的地方在于，他们的传参列表不同

call是一个参数一个参数的传，  apply是传入一个数组

```javascript
const obj = {
    name: 'caolei',
    age: 20
}

function say(num){
    console.log(this);
}

say();    //函数直接执行，this是指向window的

say.call(obj, 20)  //这样调用后，this指向obj
say.apply(obj, [20])
```



## 2.Bind方法

1. 函数调用bind的时候返回一个新的函数，只有在执行这个新的函数的时候才会改变This的指向。

2. 多次bind无效，只按第一次的算。

3. bind的特性

   3.1 会返回一个函数

   3.2 可以改变this指向， 可以传参数

   3.3 当通过new 函数执行的时候，不在指向 传递过来的参数obj,

```javascript
const obj = {
    name: 'caolei'
}

function say(){
    console.log(this)
}

let newSay = say.bind(obj);

newSay();    //this指向obj
```

```javascript
//模拟bind的实现
Function.prototype.myBind = function(target, ...args){
    if(typeof this !== 'function'){
        throw new Error('the call value must be a function');
    }
    target = target || window;
    const self = this;
    const temp = function(){};
    const foo = function(){
        return self.apply(this instanceof temp? this : target, [...args, ...arguments]);
        
        //这里的this instanceof self?  注解：
        //this指的是调用这个myBind函数的函数， 看他的原型是否出现在  由foo函数的创建的实例对象的原型链上
    }
    temp.prototype = self.prototype;
    foo.prototype = new temp();
    return foo;
}

//instanceof 是用于检测构造函数的prototype是否出现在某个实例对象的原型链上
```



## 3.箭头函数

1.箭头函数本身是没有this的， 他的this指向他的上一层this的指向

```javascript
let obj = {
    count: 0,
    add: function () {
      setTimeout(function () {
        this.count++;
        console.log(this.count);
      }, 1000);
     },
};

obj.add();
//值应该会是NaN, 因为this指向window


let obj2 = {
    count: 0,
    add: function(){
        setTimeout(() => {
            this.count++;
            console.log(this.count);
        },1000)
    }
}

obj2.add();

//值是1， 因为this被箭头函数改变了
```





## 4.call和apply和bind的区别

1. bind他是返回一个函数，在后面调用的时候灵活性更高，如果你想改变this的指向，那么就用函数执行，不想改变的话就用new 函数