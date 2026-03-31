# ajax和axios和fetch的区别

## 1.ajax

1. async javascript and xml     异步的javascript和xml

2. 传统的ajax指的是XMLHTTPRequest， ajax这个技术，它能够在不重新加载整个页面的情况下，对网页的某部分进行更新。

3. ajax的特点：

   3.1. 本身是针对MVC的编程， 不符合现在前端MVVM的浪潮

   3.2. 是基于原生的XHR开发的，XHR本身的架构就不够清晰，把所有的功能都集中到了一个对象身上

   3.3. jQuery整个项目太大， 单独的使用ajax却要引入整个jQuery非常的不合理(采取个性化打包的方案又不能享受CDN服务)



## 2.axios

1. axios是一个基于Promise用于浏览器和nodejs的HTTP库， 本质上也是对原生的XHR的封装，只不过他是promise的实现版， 符合最新的ES规范

2. 支持Promise API

3. 客户端支持防止CSRF

   CSRF：跨站请求伪造，  就是让您的每一个请求都带一个从cookie中拿到的key值，根据浏览器的同源策略，假冒的网站是拿不到你cookie中的key值的， 这样， 后台就可以轻松辨别出这个请求是否是用户在假冒网站上的误导输入， 从而采取正确的策略。

   4. 提供了一些并发请求的接口



## 3.fetch API

1. Fetch基于promise设计的，  代码结构比ajax简单许多
2. Fetch他不是ajax的进一步封装， 而是原生js,   没有使用XMLHTTPRequest
3. 更详细更方便的方法

