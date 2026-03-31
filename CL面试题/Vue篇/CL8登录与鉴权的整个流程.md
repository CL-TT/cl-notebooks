# 登录与鉴权的整个流程

# 1.登录的流程

![login](D:\CL武林秘籍\CLProjectImg\CLMy-Site\login.png)

![login-progress2](D:\CL武林秘籍\CLProjectImg\CLMy-Site\login-progress2.png)

```js
1. 登录，输入账号和密码，

1.1. 如果账号和密码错误 => 还在登录页，提示错误信息，重新登录
1.2. 如果账号密码正确 => 跳转到之前尝试的页面(有些页面需要鉴权，需要登录过后才能跳转)，如果没有尝试的页面则跳转到首页

2. 登录成功  登录的部分则会显示 【用户名  登出】

2.1. 如果点击登出 => 则清空缓存，页面重新跳转到登录页面

3. 每一次刷新页面，都会去请求，whoami方法，去跟后端确认，此前有没有登录过

3.1. 如果登录过 => 跳转到当前页
3.2. 如果没有登录过 => 则跳转到登录页进行登录
```



```js
//我们一般会在仓库中保留2个数据
export default {
    state: {
        loading: false,    //是否在加载中
        user: null,        //登录成功后返回的用户信息
    },
    
    getters: {
        //有一个计算属性status来专门的标识，现在处于什么状态，
        //一共三种状态，loading: 加载中   login: 已登录   unlogin: 未登录 
        status(state){
            if(state.loading){
                return 'loading';
            }else if(state.user){
                return 'login';
            }else{
                return 'unlogin'
            }
        }
    }
}
```



## 2.鉴权的流程

![power-progress](D:\CL武林秘籍\CLProjectImg\CLMy-Site\power-progress.png)