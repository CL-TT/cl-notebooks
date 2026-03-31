## 组件之间的双向绑定

## 1.组件之间的双向绑定的理念

组件之间的双向绑定   就是用子组件的表单元素，  用父组件的数据，   进行相关联



## 2.演示

1. 原来还是:value + @input事件， 多了一个$emit()让父组件去改变属性值
2. 如果用v-model的话， 在组件上<my-cmp1 v-model="value"></my-cmp1>, 他默认会绑定根元素,  有很大的局限性，并且使用v-model来绑定只能是绑定表单元素的value属性和input方法， 所以如果还需要指定其他的属性就需要借助model属性
3. 另一种方式就是利用v-bind的修饰符sync来进行数据的双向绑定， 他绑定的方法是this.$emit('update:value')

```html
<div id="app">
    <div>我是父组件</div>
    
    <my-cmp1 :value="value" @changevalue="changeValue"></my-cmp1>   //原始的写法
    <my-cmp1 v-model="value"></my-cmp1>   //v-model的写法
    
    <my-cmp2 v-model="checked"></my-cmp2>
    
    <my-cmp3 :value.sync="value"></my-cmp3>
</div>

<template id="my-cmp1">
    <div>
       <div>我是子组件my-cmp1</div>
        
        //子组件input元素绑定属性value，监听input事件，这是没用魔法之前的写法，原始写法
        <input :value="value"  @input="changeVal">
    </div>
</template>

//组件2来演示model属性指定特定的属性和方法
<template id="my-cmp2">
    <input type="checkbox" :checked="checked" @change="changeCheck">
</template>

//组件3来演示sync进行数据的双向绑定
<template id="my-cmp3">
    <input type="text" :value="value" @input="changeVal">
</template>

<script>
    const app = new Vue({
        el: '#app',
        data: {
            value: 'caolei'
        },
        methods: {
            changeValue(val){
                this.value = val;
            }
        }
        components: {
            //组件1
            'my-cmp1': {
                template: '#my-cmp1',
                props: {
                    value: {
                        type: String,
                        default: 'caolei',
                        required: true
                    }
                },
                methods: {
                    changeVal(e){
                        this.$emit('changevalue', e.target.value);
                    }
                }
            },
                
            //组件2， model属性绑定表单元素的其他属性
            'my-cmp2': {
                template: '#my-cmp2',
                props: {
                    checked: {
                        type: Boolean
                    }
                },
                methods: {
                    changeCheck(){
                        this.$emit('change', e.target.checked);
                    }
                },
                model: {
                    prop: 'checked',   //指定表单元素特定的属性
                    event: 'change'    //指定表单元素特定的事件
                }
            },
                
            //组件3来演示sync的数据双向绑定
            'my-cmp3': {
                template: '#my-cmp3',
                props: {
                    value: {
                        type: String
                    }
                },
                methods: {
                    changeVal(e){
                        this.$emit('update:value', e.target.value);
                    }
                }
            }
        }
    })
</script>
```





## 3. v-model和sync修饰符的区别

1. 两者都是用于实现双向数据传递的，实现方式都是语法糖，最终通过 ``prop`` + ``事件`` 来达成目的。

   v-model      prop + event

   sync             prop + update:prop

​    2.vue 1.x 的 .sync 和 v-model 是完全两个东西，vue 2.3 之后可以理解为一类特性，使用场景略微有区别

​    3.当一个组件对外只暴露一个受控的状态，且都符合统一标准的时候，我们会使用v-model来处理。

​     .sync则更为灵活，凡是需要双向数据传递时，都可以去使用