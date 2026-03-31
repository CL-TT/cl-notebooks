# This详解

1. 在全局作用域中，this关键字固定指向全局对象。
2. 在函数作用域中，取决于函数是如何被调用的
   1. 函数直接调用，this指向全局对象
   2. 通过一个对象的属性调用，格式为```对象.属性()```或```对象["属性"]()```，this指向对象

```javascript
function a(){
    console.log(this);
}

a();    //会打印window

var obj = {
    name: 'caolei',
    eat: function(){
        console.log(this);
    }
}
obj.eat();     //会指向obj对象

var obj2 = {
    age: 20,
    run: function(){
        console.log(this);
    },
    people:{
        drink: function(){
            console.log(this);
        }
    }
}
obj2.run();    //会打印obj2这个对象
var b = obj2.run;
b();   //会打印window

obj2.people.drink();    //会打印people这个对象
```

