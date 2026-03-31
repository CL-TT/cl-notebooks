# 跨域之CORS方式详解

## 1.概述

当浏览器运行了一段ajax代码(无论是使用XMLHTTPREQUEST还是使用fetch API)，浏览器会首先判断他属于哪一种请求模式



## 2.请求模式

### 2.1简单请求

当请求同时满足以下条件时，浏览器会认为他是一个简单的请求

```js
1. 请求方法属于下面的一种
get 、post 、head

2. 请求头仅包含安全的字段，常见的安全字段如下
Accept: 表示客户端希望接收的数据类型
Accept-Language: 表示客户端支持的语言格式比如中文还是英文, 
Content-Type: 发送端发送的实体数据的数据类型
Content-Language, DPR, DownLink, Save-Data, ViewPort-Width, width

3. 请求头如果包含Content-Type,仅限下面的值之一
text/plain                         : 纯文本
multipart/form-data                : 表单上传文件
application/x-www-form-urlencoded  : 最常见POST提交数据的方式。浏览器的原生form表单

如果以上三个条件同时满足，浏览器判定为简单请求


4. 当浏览器判定某个ajax跨域请求是简单请求时，会发生以下的事情

4.1 浏览器端，请求头中会自动添加origin字段,这个字段的内容是当前页面的源地址 ：协议+域名+端口号
比如说，在页面http://my.com/index.html中有以下代码造成了跨域

//简单请求 fetch("http://crossdomain.com/api/news");
请求发出后，请求头会是下面的格式
GET /api/news/ HHTP/1.1
Host: crossdomain.com
Connection: keep-alive
...
Referer: http://my.com/index.html
Origin: http://my.com     //这个字段会告诉服务器，是哪一个源地址在进行跨域请求

4.2 服务器端：服务器响应头中应包含Access-Control-Allow-Origin这个字段
当服务器收到请求后，如果允许该请求跨域访问，需要在响应头中添加Access-Control-Allow-Origin字段, 这个字段的值可以是 * 表示什么人我都允许访问， 或者是具体的源 比如http://my.com 表示我就允许你访问， 强烈推荐响应具体的源
服务器做出如下响应：
HTTP/1.1 200 OK
Date: Tue, 21 Apr 2020 08:03:35 GMT
...
Access-Control-Allow-Origin: http://my.com
...
浏览器看到服务器允许自己访问之后，于是他顺利的把响应交给js，完成后续操作

```



### 2.2需要预检的请求

```js
简单的请求对服务器的威胁不大，所以允许使用上述的方式交互即可完成，但是如果浏览器不认为这是一种简单的请求，就会按照下面的流程的进行

1. 浏览器发送预检请求，询问服务器是否允许
2. 服务器允许
3. 浏览器发送真实的请求
4. 服务器完成真实的响应

比如在http://my.com/index.html中有以下代码造成跨域

fetch('http://crossdomain.com/api/user', {
    method: 'post',
    headers: {
        a: 1,
        b: 2,
        content-type: "application/json"
    },
    body: JSON.stringify({name: '曹磊', age: 20})
})

浏览器发现这不是一个简单的请求，而是一个预检请求，则会按照下面的流程与服务器交互

1. 浏览器发送预检请求，询问服务器是否允许, 预检请求是不包含请求体的，只有请求头
OPTIONS /api/user HTTP/1.1             //请求方法为options
Host: crossdomain.com
...
origin: http://my.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: a, b, content-type

2. 服务器允许， 服务器收到预检请求后，可以检查预检请求中包含的信息，如果允许下面的请求，则需要响应下面的消息格式
HHTP/1.1 200 OK
Date: Tue, 21 Apr 2020 08:03:35 GMT
...
Access-Control-Allow-Origin: http://my.com
Access-Control-Allow-Methods: POST
Access-Control-Allow-Headers: a, b, content-type
Access-Control-Max-Age: 86400        //在这段时间内你可以不用在发预检请求告诉我了
服务器说，我允许你用post方法啊，我允许你在请求头使用这些字段

3. 服务器允许后需要做的事情，不需要响应任何的消息体，只需要在响应头中添加
Access-Control-Allow-Origin: 和简单的请求一样，表示允许的源
Access-Control-Allow-Methods: 表示允许的后续真实的请求方法
Access-Control-Allow-Headers: 表示允许改动的请求头
Access-Control-Max-Age: 告诉浏览器，多少秒内，对于同样的请求源，方法， 请求头，都不需要在发送预检请求

4. 浏览器发送真实的请求， 预检请求被服务器允许之后，浏览器就会发送真实的请求了
POST /api/user HTTP/1.1
Host: crossdomain.com
Connection: keep-alive
Referer: http://my.com/index.html
Origin: http://my.com

5. 服务器响应真实请求
HTTP/1.1 200 OK
Date: Tue, 21, Apr 2020 08:03:35 GMT
...
Access-Control-Allow-Origin
```



### 2.3附带身份凭证的请求

```js
默认情况下，ajax的跨域并不会附带cookie，这样一来，某些需要权限的操作就无法进行

//xmlhttprequest
const xhr = new XMLHttpRequest();
xhr.withCredentials = true

//fetch api
fetch(url, {
    credentials: 'include'
})

做了如上操作。该跨域的ajax请求就是一个附带身份凭证的请求，当一个请求需要附带cookie时，无论他是简单的请求，还是预检的请求，都会在请求头中添加cookie字段， 服务器在响应时，需要明确告诉客户端，服务器允许这样的凭据

1. 只需要在响应头添加Access-Control-Allow-Credentials: true
2. 若一个附带身份凭证的请求，服务器没有明确告知，浏览器仍然视为跨域被拒绝
3. 对于附带身份凭证的请求，服务器不得设置Access-Control-Allow-Origin的值为 *
    
4. 在跨域访问的时候，客户端的js只能拿到一些最基本的响应头，如果要访问其他的头，则需要服务器设置本响应头
Access-Control-Expose-Headers: name1, name2
这样js就能够访问指定的响应头了
```

