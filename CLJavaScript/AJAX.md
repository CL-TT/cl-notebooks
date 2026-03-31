# AJAX

## 1.解释和定义

async javascript and xml 是一种异步的 JavaScript 和 xml

是一种创建交互式，快速动态网页应用的网页开发技术，无需重新加载整个页面的情况下，就可以更新部分网页的技术。

## 2.ajax 工作原理

![](D:\CL武林秘籍\画图\ajax原理图.png)

## 3.原生 js 实现 ajax

```javascript
function ajax(options) {
  //对传进来数据的处理
  var options = options || {}, //进行容错，没传就默认是空对象
    url = options.url, //传进来的地址
    type = (options.type || "GET").toUpperCase(), //发送请求的类型
    dataType = options.dataType || "json", //返回数据的类型，默认为json
    data = formatData(options.data || {}), //对数据进行处理
    async = options.async || true, //是否异步
    xmlhttp = null;

  //data这个数据进行处理
  function formatData(data) {
    var arr = [];
    for (var prop in data) {
      arr.push(encodeURIComponent(prop) + "=" + encodeURIComponent(data[prop]));
    }
    arr.push(("v=" + Math.random()).replace(".", ""));
    return arr.join("&");
  }

  //初始化连接对象
  if (window.XMLHttpRequest) {
    xmlhttp = new XMLHttpRequest();
  } else {
    xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
  }

  //建立连接
  if (type == "get") {
    //如果是get请求
    xmlhttp.open(type, url + "?" + data, async);
    xmlhttp.send();
  } else if (type == "post") {
    //如果是post请求
    xmlhttp.open(type, url, async);
    xmlhttp.setRequestHeader(
      "Content-Type",
      "application/x-www-form-urlencoded"
    );
    xmlhttp.send(data);
  }

  //请求超时操作
  setTimeout(function () {
    if (xmlhttp.readyState !== 4) {
      xmlhttp.abort(); //断开这次ajax请求
    }
  }, options.timeout || 1000);

  //接收数据
  xmlhttp.onreadystatechange = function () {
    if (xmlhttp.readyState === 4) {
      //请求完成
      var status = xmlhttp.status;
      if ((status >= 200 && status < 300) || status == 304) {
        options.success && options.success(xmlhttp.responseText);
      } else {
        options.error && options.error(status);
      }
    }
  };

  //readyState 是返回XMLHttp请求的当前状态
  //0 代表请求未初始化
  //1 代表服务器连接已建立
  //2 代表请求已接收
  //3 代表请求正在处理
  //4 代表请求完成

  //status 状态码
  //200状态码 代表请求成功
  //304 你所请求的资源未修改， 客户端通常会缓存你所访问过的资源，请求相同资源时，会先到缓存中找
}

function request(params){
    // 在请求之前做一点什么
    
    ajax(params)
}
```
