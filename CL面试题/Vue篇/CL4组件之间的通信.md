# 组件之间的通信

1.父访问子总结

1. props
2. $children
3. ref特性

2.子访问父

1. $emit
2. $root,    $parent
3. 依赖注入

3.同级之间的访问

1. 事件总线  eventBus





## 1.组件之间的通信之父传子props

子组件通过props属性进行接收

```html
1. 想把父组件的name属性传给子组件

<div id="app">
    <div>我是父组件</div>
    
    <my-cmp1 :name="name"></my-cmp1>
</div>

<template id="my-cmp1">
    <div>
        <div>我是子组件my-cmp1</div>
        <div>接收从父组件传过来的属性 {{ name }}</div>
    </div>
</template>

<script>
    const app = new Vue({
        el: '#app',
        data: {
            name: 'caolei'
        },
        components: {
            "my-cmp1": {
                template: '#my-cmp1',
                props: {
                    //对name属性的校验
                    name: {
                        type: String,
                        default: 'haha',
                        required: true
                    }
                }
            }
        }
    })
</script>
```



#### 2.子组件为什么不能修改父组件传递的prop?

首先vue在组件之间的通信的设计理念是单向数据流， 因为父组件的这个属性有可能会给到多个子组件使用，如果子组件可以修改属性的话，那么就不知道到底是谁修改属性，如果发生错误那么就很难追踪到错误到底发生在哪，并且使得数据流变得难以理解



## 2.组件之间的通信之子传父$emit

通过this.$emit('自定义函数'， 参数)来进行传值

```html
<div id="app">
    <div>我是父组件</div>
    
    <my-cmp2 @getage="getAge" @getmethod="getMethod"></my-cmp1>
    
    <div>接收从子组件传递过来的属性值age  {{ age }}</div>
</div>

<template id="my-cmp2">
    <div>
        <div>我是子组件my-cmp2</div>
        
        <button @click="postAge">点击过后子组件把值传递给父组件</button>
        <button @click="postMethod">点击过后子组件把方法传递给父组件</button>
    </div>
</template>

<script>
    const app = new Vue({
        el: '#app',
        data: {
            age: ''
        },
        methods: {
            //父组件接收从子组件传递过来的属性age
            getAge(age){
                this.age = age;
            },
            
            //父组件接收从子组件传递过来的方法
            getMethod(){
                this.say = say;
            }
        }
        components: {
            "my-cmp2": {
                template: '#my-cmp2',
                data(){
                    return {
                        age: 20
                    }
                },
                methods: {
                    postAge(){
                        //传递属性
                        this.$emit('getage', this.age);
                    },
                        
                    postMethod(){
                        //传递方法
                        this.$emit('getmethod', this.say);
                    },
                        
                    say(){
                        console.log('我是子组件的say方法');
                    }
                }
            }
        }
    })
</script>
```



## 3.访问父组件的属性的另外几种方式   子访父   $root  $parent

1. $root  访问根实例中的属性，每一个子组件可以根据$root来访问根实例
2. $parent  访问父组件中的属性

   1. 这种方式是父组件向子组件传递数据的另一种形式， 但是要谨慎使用

   2. 可以获取父组件的数据，子组件可以使用
   3. 但是这种方法，如果组件嵌套多了那么就会很难调式和理解
   4. 不能指定给哪一个子组件使用
3. 注意经过这两种方式得来的属性，数据都不是响应式的



## 4.访问子组件的属性  父访子  $children

1. this.$children 访问子组件中的属性



## 5.依赖注入 还是 子访父 的一种 provide  inject

通过依赖注入这种方式，子组件得到的数据同样不是响应式的

缺点：

依赖注入还是有负面影响的。它将你应用程序中的组件与它们当前的组织方式耦合起来，使重构变得更加困难

```html
<div id="app">
    <my-cmp1>
        <my-cmp2></my-cmp2>
    </my-cmp1>
</div>

<template id="my-cmp1">
    <div>
        <slot></slot>
    </div>
</template>

<template id="my-cmp2">
    <div>
        <div>我是子组件my-cmp2</div>
    </div>
</template>

<script>
    const app = new Vue({
        el: '#app',
        data: {
            name: 'caolei',
            age: 20
        },
        
        //依赖注入,  父组件想暴露给子组件使用的属性
        provide(){
            return {
                name: this.name,
                age: this.age
            }
        }
        
        components: {
            'my-cmp1': {
               //依赖注入，子组件想使用父组件暴露出来的属性
               inject: ['name', 'age']
            },
            
            'my-cmp2': {
                inject: ['name', 'age']
            }
        }
    })
</script>
```





## 5.ref特性 访问子组件的一种方式   

```html
<div id="app">
    <my-cmp1 ref="mycmp1"></my-cmp1>
</div>

<template id="my-cmp1">
    <div>
        我是子组件my-cmp1
    </div>
</template>

<script>
    const app = new Vue({
        el: '#app',
        data: {
            
        },
        mounted(){
            //父组件通过ref访问子组件
            const mycmp1 = this.$refs.mycmp1;
            
            console.log('访问子组件的属性', mycmp1.name);
            
            console.log('访问子组件的方法', mycmp1.say);
            
            mycmp1.say();
        },
        components: {
            'my-cmp1': {
                template: '#my-cmp1',
                data(){
                    return {
                        name: 'caolei'
                    }
                },
                methods: {
                    say(){
                        console.log('aa');
                    }
                }
            }
        }
    })
</script>
```





## 6.同级组件之间的通信 eventBus(事件总线)

```html
<div id="app">
    <my-cmp1></my-cmp1>
    
    <my-cmp2></my-cmp2>
</div>

<script>
    const app = new Vue({
        el: '#app'
    })
    
    //第一步，在Vue的原型上加上$bus实例
    Vue.prototype.$bus = new Vue();
    
    Vue.component('my-cmp1', {
        template: `
           <div>
              我是子组件my-cmp1
              
              <button @click="postProp">点击我可以传递属性给兄弟组件</button>
           </div>
        `,
        data(){
            return {
                name: 'my-cmp1'
            }
        },
        methods: {
            postProp(){
                //第二步产生一个bus事件
                this.$bus.$emit('bus', this.name);
            }
        }
    })
    
    Vue.component('my-cmp2', {
        template: `
           <div></div>
        `,
        data(){
            return {
                name: 'my-cmp2'
            }
        },
        mounted(){
            //第三步监听这个bus事
            this.$bus.$on('bus', (val) => {
                console.log('接收兄弟组件传过来的属性', val);
            })
        }
    })
</script>
```

### 事件总线的原理

1. 提供监听某个事件的接口
2. 提供取消监听的接口
3. 触发事件的接口
4. 触发事件后会自动通知监听者

```js
1. EventBus是消息传递的一种方式，基于一个消息中心， 订阅和发布消息的模式。

export default{
    // 相当于订阅消息， eventName相当于订阅的消息名称， handle订阅的消息
    $on(eventName, handle){
        
    },
    
    // 相当于发布消息，eventName消息名称，params额外的参数  
    $emit(eventName, params){
        
    },
    
    // 相当于移除消息
    $off(){
        
    }
}
```

