# 网站运行机制

## 1. 名词解释

1. 域名：我们在上网通常会搜 www.xxx.com,  这个就叫域名。
2. ip地址：ip地址通常是由数字组成 比如 22.33.44.55，一个域名对应着一个ip地址
3. 服务器：类似于平常的电脑，但他的主要功能是为我们提供服务的
4. DNS服务器：这个服务器保存着域名和ip地址之间的映射关系     www.xxx.com   =>    22.33.44.55



## 2. 首先在网站输入一个网址，按下回车会发生什么呢？

1. 首先会看浏览器的缓存有没有这个域名，如果有的话，就返回其对应的ip地址
2. 如果没有的话，就看本机的host有没有这个域名(本机的host是没有缓存的)
3. 如果还没有的话，就看自家的路由器有没有 
4. 如果还是没有的话，就会向上一层路由器进行寻找，
5. 如果还是没有，就会向城市的DNS服务器进行寻找，直到在Global这一级的DNS服务器都没找到才算结束



## 3. 自己执行的流程

```js
1. scp dist.zip root@www.clei.top:~/

2. mv dist.zip /home/nginx

3. unzip dist.zip   rm -rf dist.zip

4. mv dist clblog

5. scp CLPersonalBlog2.0.zip root@www.clei.top:~/

6. mv CLPersonalBlog2.0.zip /home/nginx

7. unzip CLPersonalBlog2.0.zip  rm -rf CLPersonalBlog2.0.zip

8. cd /CLPersonalBlog2.0

9. npm install

10. pm2 start index.js 
```

