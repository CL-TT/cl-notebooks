# Function的基本使用

**所有函数都具有Function中的实例成员**

**语法：arguments：在函数中使用，获取该函数调用时，传递的所有参数**,    对应的是实参列表

**arguments是一个类数组（也称为伪数组：没有通过Array构造函数创建的类似于数组结构的对象），伪数组会缺少大量的数组实例方法**

**arguments数组中的值，会与对应的形参映射**

```javascript
function add(a, b, c){
    console.log(arguments);    //[1, 2, 3] 
    return a + b + c
}

add(1, 2, 3);

add();    //这时候arguments就不会和形参映射了
```



### 2. 实例成员

1. length属性，得到函数形参数量

```javascript
function add(a, b, c){
    
}

add.length  //会打印3，有三个形参
```



2. apply方法：调用函数，同时指定函数中的this指向，参数以数组传递

```javascript
function write(){
    console.log(this.name);
}
//这样本来会报错，因为this是指向全局的，  现在需要改变this的指向 

write();

let obj = {
    name: '曹磊'
}

write.apply(obj);    //这样write函数中的this就指向了obj
```



- call方法：调用函数，同时指定函数中的this指向，参数以列表传递
- bind方法：得到一个新函数，该函数中的this始终指向指定的值。

通常，可以利用apply、call方法，将某个伪数组转换伪真数组。

```javascript
[].slice.call(arguments);
```

