# 计算属性Computed和监听器Watch的区别

## 1.计算属性Computed和监听器Watch的区别

```html
1. 计算属性的使用场景是多个数据去影响一个数据，  而监听器的使用场景是一个数据去影响多个数据

2. 计算属性中最好不用去使用异步编程，  而在watch中则可以使用异步编程

3. 为什么在watch中可以使用异步编程，而在计算属性中不可以呢?
```



## 2.计算属性Computed

```html
1. 什么是计算属性？

计算属性可以理解为能够在里面写一些计算逻辑的属性，为的就是能够减少模板中的计算逻辑

2. 计算属性与方法的区别

 2.1 计算属性会把返回的结果进行缓存，如果依赖的值没有发生变化，那么就不会重新计算这个属性
 2.2 方法中只要改变的属性在页面上进行渲染，无论这个属性是否在方法中有依赖，都会重新运行这个方法，及其消耗性能

3. 计算属性的其他写法

计算属性除了写成一个方法外，还可以写成一个对象，对象中有两个属性get和set，这两个属性皆为函数

msg: {
  set(){
    set函数在给计算属性重新赋值时会执行。
    参数：为被重新设置的值。
    set的this，被自动绑定为Vue实例。
  },

  get(){
    获取某一个计算属性时，就会执行get函数
  }
}



<div id="app">
   <div>{{ name }}</div>
   <div>班级要去旅游，班上有{{ num }}个人，票价为每个人{{ money }}元  求这次旅游要花费多少元</div>
   <div>可以用计算属性 一共要花费{{ sum }}元</div>
   <div>方法计算 一共要花费{{ all() }}元</div>
</div>

<script>
    const app = new Vue({
        el: '#app',
        data: {
            name: 'caolei',
            num: 20,
            money: 100
        },
        methods: {
            all(){
                console.log('我是方法');
                return this.num * this.money;
            }
        },
        computed: {
            sum(){
                console.log('我是计算属性');
                return this.num + this.money;
            },
            
            allMoney: {
                set(){
                    console.log('我是set方法');
                },
                
                get(){
                    console.log('我是get方法');
                    return this.num * this.money
                }
            }
        }
    })
</script>
```





## 3.监听器Watch

```html
1. 什么是监听器Watch？

监听器会监听data中的数据变化，只要数据改变就会执行对应的逻辑

2. 监听器Watch的使用

  2.1 函数的形式

  2.2 字符串的形式

  2.3 对象的形式

  2.4 数组的形式

  2.5 实例方法使用$watch

<script>
    const app = new Vue({
        el: '#app',
        
        data: {
            name: 'caolei'
        },
        
        methods: {
            watchName(){
                console.log('我监听了name属性');
            }
        },
        
        watch: {
            //第一种方式，函数形式
            name(newVal, oldVal){
                console.log('我监听了name属性');
            },
            
            //第二种方式，字符串形式
            name: 'watchName',
            
            //第三种方式，对象的形式
            name: {
                handler: function(){
                    console.log('我监听了name属性');
                },
                
                deep: true,    //是否开启深度监听，针对对象属性而言
                
                immediate: true,  //不用等到数据改变，就会执行一次回调函数
            }
        }
    })
    
    //实例方法使用watch来监听数据。
    const unwatch = app.$watch('name', function(newVal, oldVal){
        console.log('我在监听name属性');
        
        unwatch();   //在配置了immediate为true的情况下，是不能使用unwatch的, 会报错
    }, {
        deep: true,
        immediate: true
    })
</script>

```

