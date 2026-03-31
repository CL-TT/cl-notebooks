# 变量的存放以及垃圾回收机制

## 1. 原始值的存放

```javascript
var a = 3;
var b = 'string';
//  为变量a开辟一个空间， 把3存放进去，  存放的就是具体的值
```

![](D:\CL武林秘籍\画图\原始值的存放原理.png)

## 2. 引用值的存放

```javascript
var obj = {
    name: 'caolei',
    age: 20,
    friends: {
        name: 'tt',
        age: 30
    }
}

//同样会为变量obj开辟一个存储空间，但是这个存放的是这个对象的访问地址
//js执行机制会为这个对象字面量重新开辟一个存储空间来存放这个对象， obj存放的地址指向这个存储空间
```

<img src="D:\CL武林秘籍\画图\引用值的存放原理.png" style="zoom:140%;" />

## 3. 对象引用的练习

```javascript
var obj1 = {
    name: 'caolei',
    age: 20,
    friends: {
        sex: '男'
    }
}

var obj2 = obj1;

obj2.name = 'tt';

console.log(obj1.name, obj2.name); 
//会打印 tt   tt
//因为  var obj2 = obj1;   是把obj1的地址给了obj2, 所以obj2也会指向第一个对象
//所以obj2.name修改后， obj1.name也就会被修改了

var obj1 = {
    name: 'cao'
    friends: {
      age: 20
    }
}
var obj2 = obj1;
obj2.friends = {
    age: 66
}

console.log(obj1.friends.age, obj2.friends.age);
//会打印 66 66
//因为obj2会指向obj1这个对象，  obj2的friends创建了一个新对象，所以obj1中的friends也会指向这个新对象
//所以会打印 66 66
```

## 4. js中的垃圾回收机制

js引擎中有一个垃圾回收器， 会定期的发现内存中无法访问的对象， 该对象称为垃圾，垃圾回收器会在合适的时间把这些占用的内存释放。