# 导航守卫

## 1. history对象

1. listen: 添加一个监听器，监听地址的变化， 当地址发生变化时会调用传递的函数

```js
listen

1. 参数: 函数， 运行时间点， 发生在即将跳转到新页面时

2. 这个函数的参数1: location对象，记录当前的地址信息( 当前展示的页面的location )

3. 这个函数不的参数2：action, 一个字符串， 表示进入该地址的方式

#1. pop  出栈

1. 通过点击浏览器后退前进
2. 调用history.go()
3. 调用history.goBack()
4. 调用history.goForward()

#2. push  入栈
history.push()

#3. replace  替换

4. 这个函数的返回结果，是一个函数， 可以调用该函数取消监听
```

2. block: 设置一个阻塞， 并同时设置阻塞消息，当页面发生跳转时， 会进行阻塞， 并将阻塞信息传递到路由根组件的getUserConfirmation方法， 返回一个回调函数，用于取消阻塞器



## 2. 路由根组件的getUserConfirmation方法

```js
1. 参数： 是一个函数

2. 这个函数的参数1： 阻塞消息
  
#1. 字符串消息

#2. 函数：函数的返回结果是一个字符串，用于表示阻塞消息， 这个函数的参数1,location对象， 参数2，action值

3. 这个函数的参数2： 回调函数callback, 调用该函数并传递true，则表示进入到新页面， 否则不进行任何操作
  
```

![react-router](D:\CL武林秘籍\画图\react-router.png)



## 3. 封装一个路由守卫组件

```js
import { BrowserRouter as Router, withRouter } from 'react-router-dom';
import React, { Component } from 'react';

let unBlock, preLocation, curLocation, action;

class _GuardHelp extends component{
    componentDidMount(){
        //注册一个阻塞器, 一旦触发阻塞器，就会执行Router中的getUserConfirmation方法
        unBlock = this.props.history.block((location, _action) => {
            preLocation = this.props.location;  //从哪一个页面来的信息
            curlocation = location;  //要去那个页面的信息
            action = _action;  //跳转方式
            return null
        });
        
        //注册一个监听器，一旦地址发生变化，就会触发这个函数
        this.unListen = this.props.history.listen((location, action) => {
            if(this.props.onAddressChange){
                this.props.onAddressChange(this.props.location, location, action, this.unListen);
            }
        })
    }
    
    componentWillUnMount(){
        //在组件卸载的时候，清除掉阻塞器和监听器
        unBlock();
        this.unListen();
    }
    
    render(){
        return null
    }
}

// 利用withRouter是为了获取history对象
const GuardHelp = withRouter(_GuardHelp);

/**
* 这个是一个顶部路由组件
**/
class GuardRouter extends Component {
    /**
    * 在跳转之前能做什么，并且可以控制是否跳转到那个页面
    **/
    handleBeforeChange = (msg, callback) => {
        if(this.props.onBeforeChange){
            this.onBeforeChange(preLocation, curLocation, action, callback, unBlock);
        }
    }
    
    render(){
        return <Router getUserConfirmation={ this.handleBeforeChange }>
            <GuardHelp onAddressChange={ this.props.onAddressChange }/>
            { this.props.children } 
        </Router>
    }
}

export default GuardRouter;
```

```html
使用封装好的组件

import GuardRouter from ''; 
import { Route } from 'react-router-dom';

function App(){
  /**
  * 在跳转之前能做什么， 并且可以控制是否跳转
  **/
  const beforeChange = (pre, cur, action, callback, unblock) => {

  }

  /**
  * 地址改变了能做什么
  **/
  const addressChange = (pre, cur, action, unListen) => {

  }
  return <GuardRouter onBeforeChange={ beforeChange } onAddressChange={ addressChange } >
      <Route></Route>
</GuardRouter>
}
```

