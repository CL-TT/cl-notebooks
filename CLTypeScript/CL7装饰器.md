# 装饰器

## 1. 什么是装饰器，解决什么样的问题

### 1. 什么是装饰器 （js中本身就有装饰器）

装饰器就是一个普通的函数，它可以修饰类，类中的成员包括属性和方法，参数， 为这些提供元数据， 它是参与运行的



### 2. 装饰器解决什么样的问题

**装饰器用于分离关注点，为某些属性，类，参数，方法 ，提供元数据的**， 根本原因就是这些东西在定义的时候，附加的信息量有限

```ts
比如有一个用户类，类中有这么几个属性，这几个属性需要校验，常规做法可能去写一个验证方法，把这个用户当做参数传进去，这样做法的缺点就是

1. 明明是在定义这个属性的时候最清楚这个属性的情况，现在却把它分离到一个验证函数里面
2. 会造成一定的重复代码

class User {
    @require   用来描述是必填的
    @ranage(2,10)  长度在2-10之间
    name: string   // 非空，长度在2-10之间
    age: number    // 非空，长度在1-3之间
}
```



## 3. 类装饰器

**类装饰器本质上是一个函数，函数有一个参数表示类本身**

装饰器的写法  @得到一个函数

这个函数的返回值要么是void要么是一个新的类

装饰器函数的运行时间，在类定义后就直接运行

当有多个装饰器的时候，它的运行是自下往上的

```ts
// 在TS中如何约束一个变量为类
// 1. Function 但是不具体
// 2. new () => object

function test1(target: new (...args:any[]) => object):void {
    console.log('test1')
}

function test2(target: new () => object):void {
    console.log('test2')
}

@test1
@test2
class A {
    
}

// 会先打印test2, 再打印test1
```



## 4. 类成员的装饰器

属性装饰器：本质上是一个函数，他有两个参数，第一个参数，如果属性是静态成员那么第一个参数就是类本身，如果是实例成员那么就是这个类的原型，  第二个参数就是属性的名称



方法装饰器：本质上是一个函数，他有三个参数，第一个参数如上，第二个参数是方法名称，第三个参数是属性描述符（是否可写，是否可遍历，value值）

```ts
type constructor = new (...args:any[]) => object

function test(target: constructor, key: string){
    //如果是name 实例属性 target就是A的原型
    
    //如果是age 静态属性 target就是A本身
}

// 让这个方法过期
function test2(target: constructor, key: string, desc: PropertyDescriptor){
    desc.value = function () {
        consoloe.log('这个方法已经过期')
    }
}


class A {
    @test
    name: string
    
    @test
    static age: number
    
    @test2
    say():void {
        
    }
}
```



## 5. 参数装饰器

```
同样参数装饰器也是一个函数，该函数有三个参数

1. 第一个参数如果是静态方法则是类本身，如果是实例方法则是类的原型
2. 第二个参数是方法的名称
3. 第三个参数是，修饰的参数的索引
```



## 6. 工具库

1. reflect-metadata   保存元数据的

```ts
import 'reflect-metadata'


@Reflect.metadata(key, '用户A')
class A {
    @Reflect.metadata(key, '')
    name: string
}

// 获取元数据

// 获取类上的元数据
Reflect.getMetadata(key, 类名)

// 获取属性上的元数据
Reflect.getMetadata(key, 对象, 属性名)

// 判断有没有这个元数据, 和上述一样
Reflect.hasMetadata(key, ...)
```



1. class-validator   验证类的属性

```ts
import { IsNotEmpty, validator } from 'class-validator'    // 从这个库里导出不同的验证规则

class Movie {
    @IsNotEmpty({ message: '姓名不能为空' })
    public name: string
}

const m = new Movie()

validator(m).then(err => {
    
})
```



1. class-transformer  把平面对象转化为类

```ts
import { plainToInstance, Type } from 'class-transformer'

要使用Type,还需要reflect-metadata这个库，直接导入即可

class Movie {
    @Type(() => String)  // 防止平面对象中的name没有传字符串， 也被称为类型转换装饰器
    public name: string
}

const obj = {
    name: ''
}

// 第一个参数是要转什么类，第二个参数是哪一个平面对象
const t = plainToInstance(Movie, obj)



```

