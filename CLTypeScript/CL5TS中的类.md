# TS中的类

## 1. 基本使用

```ts
class User{
    readonly id: string    // 只读
    
    name: string
    
    private age: number = 20    // 私有属性
    
    pid? : string               // 可选属性
    
    constructor(name: string) {
        this.name = name
    }
}
```



## 2. 访问器

访问器的作用就是控制属性的读取和赋值

```ts
class User {
    name: string
    
    private _age: number   // 必须要给一个修饰符,私有的
    
    constructor(name: string, age: number) {
        this.name = name
        this._age = age
    }
    
    set age(value: number) {
        this._age = value
    }
    
    get age() {
        return this._age
    }
}

const u = new User('caolei', 20)

u.age = 20
u.age
```



## 3. 继承（用来描述类和类之间的关系）

1. 子类可以继承父类的属性和方法，也就是说可以使用父类的属性和方法

2. 子类可以重写父类的属性和方法， 但是不能改变它的类型

3. 一些关键字

   super: 可以在子类中调用父类的属性和方法

   protected,受保护的，只能在自己或者其子类可以访问

4. 子类的对象始终可以赋值给父类，里氏替换原则

```ts
class Tank {
  protected name:string = '坦克'
}

class MyTank extends Tank {
    say() {
        console.log(this.name) 
    }
}

const t = new MyTank()

t.say()

protected和private的区别

protected，在自己的类中和子类中都可以访问
private，是最严格的，只能在自己的类中使用
```



## 4. 抽象类

一些抽象的东西，能创建对象，但是不符合实际逻辑，只能提取公共的内容，那么这些可以被称为抽象类



1. 子类继承抽象类，那么子类要去实现抽象类中的抽象成员，是一种强约束

2. 为什么会有抽象成员

   就是因为这个属性是其他子类共有的，但是不知道他的初始值应该设为什么，只有在子类中才知晓，比如说棋子的名称，在棋子类中没有办法去设置这个名称，只有在具体的棋子类中才知道

```ts
例如，象棋中的棋子

abstract class Chess{
    abstract name:string    // 抽象成员
}

class Slodier extends Chess {
    
}
```



## 5. 静态成员

就是附着在类上的属性和方法

```ts
class Chess {
    static name:string = 'cl'    // static修饰符修饰的
}

Chess.name   // 直接通过类调用，不能通过实例调用

// 单例模式， 在某种场景下某个对象只能有一个实例

比如说象棋中的棋盘类，只能有一个棋盘

class Bord {
  private constructor() {}

  private static _bord?:Bord

  static createBord() {
    if (this._bord) {
      return this._bord
    } else {
      this._bord = new Bord()
      return this._bord
    }
  }
}

const b1 = Bord.createBord()

const b2 = Bord.createBord()

console.log(b1 === b2);
```



## 6. 接口如何和类在一起使用

接口就是为了给类赋予一种能力

```ts
// 表明有跳舞的能力

interface IDance {
    dance():void
}

class Animal {
    
}

//狗这个子类继承动物类，同时又实现了IDance这个接口，表明狗拥有跳舞的能力
class Dog extends Animal implements IDance {
    // 那么在这个类中就要实现这个dance方法
    dance() {
        console.log('狗狗跳舞了')
    }
}
```

```ts
例子

// 需求
// 有一个动物类，子类有狗，狮子，猴子，老虎
// 他们都有一些共同的属性比如说，姓名，年龄，会打招呼，种类
// 每个动物都会有一些技能
// 狮子和老虎会钻火圈，猴子走钢丝，狗会计算

abstract class Animal {
  abstract type:string

  constructor(public name:string, public age:number){}

  sayHello():void {
    console.log(`我是${this.type}, 我叫${this.name}, 我今年${this.age}岁了`);
  }
}

// 定义一个钻火圈的接口
interface IZuanFire {
  fire():void
}

interface IWalk {
  walk():void
}

interface ISmart{
  smart():void
}

/**
 * 判断这个动物或者说这个对象会不会钻火圈
 * ani: object  这个参数为什么是object而不是Animal，是因为要判断的是这个对象有没有这个能力，这个对象可以是动物，也可以是其他
 */
function isFire(ani: object): ani is IZuanFire {
  //就是判断这个对象里面有没有fire这个方法
  if (((ani as IZuanFire).fire)) {
    return true
  }
  return false
}

class Dog extends Animal implements ISmart {
  type:string = '狗'

  smart() {
    console.log(`我是${this.name}, 我会计算`);
  }
}

class Lion extends Animal implements IZuanFire {
  type:string = '狮子'

  fire() {
    console.log(`我是${this.name}，我会钻火圈`);
  }
}

class Tiger extends Animal implements IZuanFire {
  type:string = '老虎'

  fire() {
    console.log(`我是${this.name}, 我会钻火圈`);
  }
}

class Monkey extends Animal implements IWalk {
  type:string = '猴子'

  walk() {
    console.log(`我是${this.name}, 我会走钢丝`);
  }
}


const animals:Animal [] = [
  new Dog('旺财', 5),
  new Lion('辛巴', 10),
  new Tiger('小白', 17),
  new Monkey('吉吉', 8)
]

// 第一步所有的动物打招呼
// animals.forEach(item => {
//   item.sayHello()
// })

// 第二步让会钻火圈的动物表演，现在就是没有一个标识表明哪些动物会钻火圈，有了接口就可以实现了
animals.forEach(item => {
  if (isFire(item)) {
    item.fire()
  }
})
```



## 7. 索引器

TS中索引器的作用

1. 在严格的检查下，可以为类动态的添加成员
2. 可以实现动态的操作类成员

```ts
class A {
    [prop: string]: string
}

const a = new A()

a['name'] = 'caolei'
```



## 8. TS中this指向约束

1. 在配置文件里面配置noImplicitThis为true的话，表示this不允许隐式的指向any
2. 在TS中允许在书写函数的时候指定this的指向，将this作为函数的第一个参数，该参数并不是真正的参数，只起到约束作用，并且这不会出现在编译结果中

```ts
interface IUser = {
    name: string
    say(this: IUser): void
}

const obj = {
    name: 'cl',
    say() {
        console.log(this.name)
    }
}

class A {
    constructor(public name:string) {
        
    }
    
    say(this: A) {
        console.log(this.name)
    }
}
```

