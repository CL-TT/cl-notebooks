# 继承方式

## 1.传统的方式  原型链

```javascript
function Gold(){
    this.name = 'caolei';
}

function Father(){
    this.sex = 'nan';
}

Father.prototype = new Gold();

function Son(){
    this.age = 20;
}

Son.prototype = new Father();

1. 这样的继承方式
Son  >   Father   >    Gold

2. 缺点 有些属性不想继承，却也被继承下来了

3. son的原型指向了  new Father 是一个对象{ sex: '' }  Father的原型又指向了 new Gold  是一个对象 { name: '' }
所以son可以访问father，gold中的属性和方法
```





## 2.构造函数  call和apply的方式

```javascript
function Father(name, age, sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
}

function Son(name, age, sex, word){
    Father.call(this, name, age, sex);
    
    this.word = word;
}

const son = new Son('caolei', 22, 'nan', '我是帅哥');

son这个对象下面有   

son = {
    name: 'caolei',
    age: 22,
    sex: 'nan',
    word: '我是帅哥'
}

这就相当于Son这个构造函数继承了Father这个构造函数的里面的内容

缺点：
1. 只能继承构造函数里面的内容
2. 不能继承Father原型上的属性或者方法
```





## 3.共享原型

```javascript
function Father(name){
    this.name = name;
}

Father.prototype.lastName = 'kk';

function Son(age){
    this.age = age;
}

Son.prototype = Father.prototype;

const son = new Son(22);

console.log(son.lastName)  // 会打印出kk


//如果给子类的原型上添加属性或者方法
Son.prototype.prop = 'prop';

console.log(Father.prototype.prop);  // 会打印出prop

```



