# 解析ReactivityAPI

## 1. 如何获取响应式数据

| API      | 传入               | 返回           | 备注                                                         |
| -------- | ------------------ | -------------- | ------------------------------------------------------------ |
| reactive | 普通对象           | 对象代理       | 深度代理对象中的所有成员                                     |
| readonly | 普通对象或代理对象 | 对象代理       | 只能读取代理对象中的成员，不可以修改                         |
| ref      | any                | { value, ... } | 对value的访问是响应式的，如果给value的值是一个对象，则会通过reactive函数进行代理，如果已经代理过了，则直接使用代理 |
| computed | function           | { value, ... } | 当读取Value值时，会根据情况决定是否要运行函数                |

- 如果想要让一个对象变为响应式数据，可以使用`reactive`或`ref`
- 如果想要让一个对象的所有属性只读，使用`readonly`
- 如果想要让一个非对象数据变为响应式数据，使用`ref`
- 如果想要根据已知的响应式数据得到一个新的响应式数据，使用`computed`



```js
//笔试题1

import { reactive, readonly, ref, computed } from "vue";

const state = reactive({
  firstName: "Xu Ming",
  lastName: "Deng",
});
const fullName = computed(() => {
  console.log("changed");
  return `${state.lastName}, ${state.firstName}`;
});
console.log("state ready");
console.log("fullname is", fullName.value);
console.log("fullname is", fullName.value);
const imState = readonly(state);
console.log(imState === state);

const stateRef = ref(state);
console.log(stateRef.value === state);

state.firstName = "Cheng";
state.lastName = "Ji";

console.log(imState.firstName, imState.lastName);
console.log("fullname is", fullName.value);
console.log("fullname is", fullName.value);

const imState2 = readonly(stateRef);
console.log(imState2.value === stateRef.value);
```

```js
//笔试题2

//核心就是把不可修改的抛给外部，内部用可修改的

import { reactive, readonly } from 'vue';

function useUser(){
  // 在这里补全函数
  const originUser = reactive({});
    
  const user = readonly(originUser);
    
  const setUserName = (name) => {
      originUser.name = name;
  }
  
  const setUserAge = (age) => {
      originUser.age = age;
  }
    
  return {
    user, // 这是一个只读的用户对象，响应式数据，默认为一个空对象
    setUserName, // 这是一个函数，传入用户姓名，用于修改用户的名称
    setUserAge, // 这是一个函数，传入用户年龄，用户修改用户的年龄
  }
}
```



```js
//笔试题3

function useDebounce(obj, duration){
  // 在这里补全函数
    
  const originValue = reactive(obj);
    
  const value = readonly(originValue);
    
  let timer = null
    
  const setValue = (newObj) => {
      cleartimeout(timer);
      timer = setTimeout(() => {
          Object.enties(newObj).forEach(([key, val]) => {
              originValue.key = val;
          })
      }, duration);
  }
    
  return {
    value, // 这里是一个只读对象，响应式数据，默认值为参数值
    setValue // 这里是一个函数，传入一个新的对象，需要把新对象中的属性混合到原始对象中，混合操作需要在duration的时间中防抖
  }
}
```



## 2. 如何监听数据变化

#### 1.watchEffect

```js
import { watchEffect } from 'vue';

const stop = watchEffect(() => {
  //该函数会立即执行
})

//通过调用stop函数，可以停止监听
stop();
```



#### 2. Watch (相当于vue2中的$watch)

```js
//1. 监听单个数据的变化

import { reactive } from 'vue';

const state = reactive({ count: 0 });

watch(() => state.count, (newValue, oldValue) => {
    //不是立即执行的，除非你进行了配置
}, options)

const countRef = ref(0);
watch(countRef, (newValue, oldValue) => {
  // ...
}, options)

// 监听多个数据的变化
watch([() => state.count, countRef], ([new1, new2], [old1, old2]) => {
  // ...
});
```

**注意：无论是`watchEffect`还是`watch`，当依赖项变化时，回调函数的运行都是异步的（微队列）**

应用：除非遇到下面的场景，否则均建议选择`watchEffect`

- 不希望回调函数一开始就执行
- 数据改变时，需要参考旧值
- 需要监控一些回调函数中不会用到的数据



## 3. 判断

| API        | 含义                                       |
| ---------- | ------------------------------------------ |
| isProxy    | 判断某个数据是否由reactive或readonly创建的 |
| isReactive | 判断某个数据是否由reactive创建的           |
| isReadonly | 判断某个数据是否是通过readonly创建的       |
| isRef      | 判断某个数据是否是一个ref对象              |



### 4. 转换

```js
1. unref => isRef(val) ? val.value : val

const state = unref(val);


2. toRef 得到一个响应式对象的某个属性的ref格式

const state2 = reactive({ name: 'caolei', age: 20 });

const nameRef = toRef(state2, 'name');

3. toRefs 把一个响应式对象的所有属性都转为ref格式的，然后包装到一个普通对象中返回

const state3 = reactive({ name: 'caolei' });

const state3Refs = toRefs(state3)

/*
stateAsRefs: not a proxy
{
  foo: { value: ... },
  bar: { value: ... }
}
*/


4. 所有的`composition function`均以`ref`的结果返回，以保证`setup`函数的返回结果中不包含`reactive`或`readonly`直接产生的数据

const state = reactive({ name: 'caolei' });

//这样数据就不具有响应式了
const obj = {
    ...state
}
```

