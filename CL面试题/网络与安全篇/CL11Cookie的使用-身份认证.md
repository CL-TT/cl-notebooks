# Cookie的使用（适用于浏览器）

## 1.概念

1. 因为http协议是无状态的，就是**服务器不知道这一次请求的人，跟之前登录请求成功的人是不是同一个人**，所以我们需要一个通行证来证明您曾经来过。

2. Cookie(4kb的大小)是浏览器中特有的一个概念，它就像浏览器的专属卡包，管理着各个网站的身份信息。

   每个cookie就相当于是属于某个网站的一个卡片，它记录了下面的信息

   

3. 客户端登录成功后，服务器会给客户端一个出入证（令牌 token）

4. 后续客户端的每次请求，都必须要附带这个出入证（令牌 token）

![cookie1](C:\Users\caolei\Pictures\笔记图片\cookie1.png)

![cookie2](C:\Users\caolei\Pictures\笔记图片\cookie2.png)

## 2.cookie具体内容

1. key：键，比如「身份编号」

2. value：值，比如袁小进的身份编号「14563D1550F2F76D69ECBF4DD54ABC95」，这有点像卡片的条形码，当然，它可以是任何信息

3. domain：域，表达这个cookie是属于哪个网站的，比如`yuanjin.tech`，表示这个cookie是属于`yuanjin.tech`这个网站的

4. path：路径，表达这个cookie是属于该网站的哪个基路径的，就好比是同一家公司不同部门会颁发不同的出入证。比如`/news`，表示这个cookie属于`/news`这个路径的。（后续详细解释）

5. secure：是否使用安全传输（后续详细解释）

6. expire：过期时间，表示该cookie在什么时候过期

当浏览器向服务器发送一个请求的时候，它会瞄一眼自己的卡包，看看哪些卡片适合附带捎给服务器

如果一个cookie**同时满足**以下条件，则这个cookie会被附带到请求中

- cookie没有过期
- cookie中的域和这次请求的域是匹配的
  - 比如cookie中的域是`yuanjin.tech`，则可以匹配的请求域是`yuanjin.tech`、`www.yuanjin.tech`、`blogs.yuanjin.tech`等等
  - 比如cookie中的域是`www.yuanjin.tech`，则只能匹配`www.yuanjin.tech`这样的请求域
  - cookie是不在乎端口的，只要域匹配即可
- cookie中的path和这次请求的path是匹配的
  - 比如cookie中的path是`/news`，则可以匹配的请求路径可以是`/news`、`/news/detail`、`/news/a/b/c`等等，但不能匹配`/blogs`
  - 如果cookie的path是`/`，可以想象，能够匹配所有的路径
- 验证cookie的安全传输
  - 如果cookie的secure属性是true，则请求协议必须是`https`，否则不会发送该cookie
  - 如果cookie的secure属性是false，则请求协议可以是`http`，也可以是`https`

如果一个cookie满足了上述的所有条件，则浏览器会把它自动加入到这次请求中



## 3.如何设置cookie

1. 服务器响应：这种模式是非常普遍的，当服务器决定给客户端颁发一个证件时，它会在响应的消息中包含cookie，浏览器会自动的把cookie保存到卡包中

2. 客户端自行设置：这种模式少见一些，不过也有可能会发生，比如用户关闭了某个广告，并选择了「以后不要再弹出」，此时就可以把这种小信息直接通过浏览器的JS代码保存到cookie中。后续请求服务器时，服务器会看到客户端不想要再次弹出广告的cookie，于是就不会再发送广告过来了。



```javascript
//1. 服务器端设置cookie，   服务器通过设置响应头来设置cookie

//set-cookie : 键=值; path=?; domain=?; expire=?; max-age=?; secure; httponly

//当这样的响应头到达客户端后，浏览器会自动的将cookie保存到卡包中，如果卡包中已经存在一模一样的卡片（其他key、path、domain相同），则会自动的覆盖之前的设置

//cookie相同， 必须是key值 path值  domain值都相同才能叫相同
```

- **path**：设置cookie的路径。如果不设置，浏览器会将其自动设置为当前请求的路径。比如，浏览器请求的地址是`/login`，服务器响应了一个`set-cookie: a=1`，浏览器会将该cookie的path设置为请求的路径`/login`
- **domain**：设置cookie的域。如果不设置，浏览器会自动将其设置为当前的请求域，比如，浏览器请求的地址是`http://www.yuanjin.tech`，服务器响应了一个`set-cookie: a=1`，浏览器会将该cookie的domain设置为请求的域`www.yuanjin.tech`
  - 这里值得注意的是，如果服务器响应了一个无效的域，浏览器是不认的
  - 什么是无效的域？就是响应的域连根域都不一样。比如，浏览器请求的域是`yuanjin.tech`，服务器响应的cookie是`set-cookie: a=1; domain=baidu.com`，这样的域浏览器是不认的。
  - 如果浏览器连这样的情况都允许，就意味着张三的服务器，有权利给用户一个cookie，用于访问李四的服务器，这会造成很多安全性的问题
- **expire**：设置cookie的过期时间。这里必须是一个有效的GMT时间，即格林威治标准时间字符串，比如`Fri, 17 Apr 2020 09:35:59 GMT`，表示格林威治时间的`2020-04-17 09:35:59`，即北京时间的`2020-04-17 17:35:59`。当客户端的时间达到这个时间点后，会自动销毁该cookie。
- **max-age**：设置cookie的相对有效期。expire和max-age通常仅设置一个即可。比如设置`max-age`为`1000`，浏览器在添加cookie时，会自动设置它的`expire`为当前时间加上1000秒，作为过期时间。
  - 如果不设置expire，又没有设置max-age，则表示会话结束后过期。
  - 对于大部分浏览器而言，关闭所有浏览器窗口意味着会话结束。
  - 删除cookie那么就设置过期时间
- **secure**：设置cookie是否是安全连接。如果设置了该值，则表示该cookie后续只能随着`https`请求发送。如果不设置，则表示该cookie会随着所有请求发送。
- httponly：设置cookie是否仅能用于传输。如果设置了该值，表示该cookie仅能用于传输，而不允许在客户端通过JS获取，这对防止跨站脚本攻击（XSS）会很有用。 
  - 关于如何通过JS获取，后续会讲解
  - 关于什么是XSS，不在本文讨论范围

```javascript
//2. 浏览器设置cookie

//document.cookie = "键=值; path=?; domain=?; expire=?; max-age=?; secure";

document.cookie = 'id=4;expire=30*24*60*60'  // 设置cookie
```

- 没有httponly。因为httponly本来就是为了限制在客户端访问的，既然你是在客户端配置，自然失去了限制的意义。
- path的默认值。在服务器端设置cookie时，如果没有写path，使用的是请求的path。而在客户端设置cookie时，也许根本没有请求发生。因此，path在客户端设置时的默认值是当前网页的path
- domain的默认值。和path同理，客户端设置时的默认值是当前网页的domain
- 其他：一样
- 删除cookie：和服务器也一样，修改cookie的过期时间即可



## 4.总结

如果把cookie放入到登录场景，具体流程如下

1. #### 登录请求

   1. 浏览器发送登录请求到服务器
   2. 服务器验证账号和密码是否匹配，如果不匹配则响应错误信息，如果匹配，服务器会自动的在响应头中设置cookie，并附带登录验证信息
   3. 客户端收到cookie,浏览器自动记录下来

2. #### 后续请求

   1. 如果后续使用其他接口访问服务器，浏览器会自动的把cookie信息附带到请求头中
   2. 服务器先获取cookie，验证cookie中的信息是否正确，如果不正确，不予以操作，如果正确，完成正常的业务流程