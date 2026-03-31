# 如何理解Vue的数据响应原理

**响应式数据的最终目标就是，当对象本身或者对象的属性发生变化时，将会运行一些函数，最常见的就是render函数，通过render函数让视图上的数据更新**



## 1.Observer (目标就是把普通的对象转化为响应式的对象)

1. Observer的功能就是把普通的对象转化为响应式对象, 下面来模拟一下这个转换的过程

```javascript
let obj = {
    name: 'caolei',
    car: {
        price: 100
    },
    arr: [1, 2, 3]
}

class Observer{
    constructor(target){
        this.walk(target);
    }
    
    walk(target){
        //如果不存在如果不是对象，直接返回
        if(!target || typeof target !== 'object') return;
        
        //遍历对象的属性
        Object.keys(target).forEach(prop => {
            this.defineReactive(target, prop, target[prop]);
        })
    }
    
    defineReactive(target, prop, value){
        new Observer(value);   //对属性的值进行递归
        
        Object.defineProperty(target, prop, {
            set(newVal){
                if(newVal == value) return
                value = newVal;
                
                把值设置成功之后要做什么？
            },
            
            get(){
                return value;
                
                读取值之后要做什么？
            },
            
            enumerable: true,     //可枚举
            
            configurable: true,   //可配置
        })
    }
}

new Observer(obj);
```

![xiangying](D:\CL武林秘籍\画图\xiangying.png)

2. 要注意的几个问题

   1. 如果是一个对象，再后来去添加或者删除修改属性的话，vue是监测不到的，所以他出了一个this.$set(target, prop, value)来进行操作

   2. 数组的那些方法都是变异过后的方法

      ```javascript
      arr.__proto__    ->    自己定义的对象的prototype   self.__proto__     ->     Array.prototype
      ```

      



## 2.Dependency(依赖)

现在还有两个问题没有解决，就是当你读取完属性，和重新赋值属性之后要干什么？

1. vue会为响应式对象中的每一个属性，对象本身，数组本身创建一个dep实例，每一个dep实例都能做以下两件事
   1. 记录依赖，是谁在用我
   2. 派发更新，我变了，我要通知那些用到我的人
2. 当读取响应式对象的某个属性时，他会进行依赖收集，有人用到了我，当改变某个属性时，他会派发更新，那些用我的人听好啦，我改变了

```vue
<template>
    <div id="app">
       {{ name }}
        
       <span v-for="item in arr">{{ item }}</span> 
        
       <div>{{ temp.price }}</div> 
    </div>
</template>


<script>
    export.default {
        data(){
            return {
                name: 'caolei',
                arr: [1, 2, 3],
                temp: {
                    price: 100
                },
                count: 0
            }
        }
    }
</script>

1. 模板字符串最终会变成render函数，在执行这个render函数的时候，首先会读取name属性，然后会读取arr属性, 然后会读取到temp属性


{
  name  =>  dep( 会记录有一个render在依赖我 )
  arr:[ => dep( render )
    1  =>  dep( render )
    2  =>  dep( render )
    3  =>  dep( render )
  ]
  temp:{  =>   dep( render )
    price  =>   dep( render )
  }
  count  =>  因为没有读取count属性，所以没有dep
}
```



## 3. watcher

这里又会诞生一个问题，dep如何知道是谁在用我呢？

1. 当执行到某个render函数的时候，先不执行，先交给watcher去执行， watcher就是一个对象， 每一个这样的函数执行时都应该创建一个watcher，通过watcher去执行
2. 关键在于这里，**watcher会设置一个全局变量，比如const  currentWatcher = this;   指向本身，然后等到去执行函数的时候，有用到属性产生了依赖 dep.depend(),  那么dep就会把这个全局变量记录下来，** 表示有一个watcher用到了我这个属性
3. 当Dep进行派发更新时，他会通知之前记录的所有watcher，告诉他们，我变了
4. `watcher`首先会把`render`函数运行一次以收集依赖，于是那些在render中用到的响应式数据就会记录这个watcher。当数据变化时，dep就会通知该watcher，而watcher将重新运行render函数，从而让界面重新渲染同时重新记录当前的依赖。



## 4. Scheduler(调度器)

这里又会产生一个问题，**就是dep通知watcher之后， 一个函数可能或有多个依赖的属性产生多个dep,每个dep又通知watcher， 就会导致函数频繁执行**

比如一个交给watcher的函数，它里面用到了属性a, b, c, d,那么属性abcd都会记录依赖，下面的代码将触发四次更新, 显然不合理

```javascript
state.a = 'a';
state.b = 'b';
state.c = 'c';
state.d = 'd';
```

1. 因此，watcher收到派发更新的通知后，**实际上不是立即执行对应的函数，而是把自己交给一个叫调度器的东西， 这个调度器维护一个队列，该队列同一个watcher只会存在一次， 队列中的watcher也不是立即执行， 他会通过一个nextTick的工具方法，把这些需要执行的watcher放入到事件循环的微队列中**， nextTick的具体做法是通过promise完成的

2. 也就是说，**当响应式数据变化时，`render`函数的执行是异步的，并且在微队列中**

   ```javascript
   所以想拿到视图上更新后的数据，则需要通过this.$nexttick(() => {
       
   })
   ```

   

## 5. 总流程

![xiangying2](D:\CL武林秘籍\画图\xiangying2.png)
