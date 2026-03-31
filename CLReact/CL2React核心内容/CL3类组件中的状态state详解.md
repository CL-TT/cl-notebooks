# 类组件中的状态state详解

## 1. 组件状态(类组件独有的)

1. 组件可以自行维护的数据，只能组件自身可以更改，本质上是类组件的一个属性
2. 比如说有一个倒计时组件，这个组件的功能需要组件自己来实现，而不是外界来帮助他实现js逻辑，所以组件需要自己的数据，俗称组件状态



## 2. 初始化组件状态

```react
//1. 第一种方式
state = {
    
};

//2. 第二种方式
constructor(){
    super();
    this.state = {
        
    };
}
```



## 3. 修改组件状态方式

```react
//1. 在组件内想修改组件状态，只能通过this.setState来修改，当修改之后，会自动重新渲染这个组件

this.setState({
    
})

//这种方式就是Object.assign来复合对象中的属性值


//2. 思考
1、react中数据改变，他是怎么监测知道的?
2、react为什么要用this.setState来改变数据？

//1. 正是因为react没有使用过多的魔法，不像vue那样，vue2.0中使用了defineProperty， vue3.0中使用代理来对数据的监测

//2. 所以想要改变state状态数据， 就必须使用this.setState

3、react中的this.setState和小程序中的this.setData是不是些许相似？
```

