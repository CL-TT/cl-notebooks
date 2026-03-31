# Cookie和Session的区别， localStorage和sessionStorage的区别

## 1.Cookie和Session的区别

1. **存储位置**：cookie存储在客户端浏览器上，session的数据信息存放在服务器上
2. **存储容量**：cookie的存储容量一般在4kb左右，对于session来说他是没有上限的，但是出于对服务器的性能考虑，session内不要存放过多的东西
3. **存储方式**：cookie中只能保存字符串或者二进制数据，session是能够存储任何类型的数据的
4. **隐私策略**：cookie对客户端是可见的，别有用心的人可以分析存放在本地的cookie并进行cookie欺骗所以他是不安全的，可以设置httponly让js拿不到cookie， session存储在服务器上，所以不存在敏感信息泄漏的风险
5. **跨域支持**：cookie支持跨域名访问，session不支持跨域名访问

## 2. localStorage和sessionStorage的区别

### 1.相同点

1. localStorage和sessionStorage都是用来存储客户端临时信息的对象
2. 他们均只能存储字符串类型的对象
3. 他们不像cookie那样，都不会把数据主动的发送给服务器
4. 他们的存储限制因不同的浏览器而异，但一般在5MB到10MB之间



### 2.不同点

1. **保存数据的有效期**： localStorage中的数据是长期有效的，因此可以被用作为数据持久化

   而sessionStorage的生命周期为当前窗口或标签页，一旦窗口或者标签页被永久关闭，那么所有通过sessionStorage存储的数据也会被清空

2. **数据的作用域**：不同的浏览器是无法共享localStorage或者sessionStorage中的信息的， localStorage在相同的浏览器不同的页面间是可以共享的(页面属于相同域名和端口)，但是sessionStorage相同的浏览器不同的页面是无法共享信息的