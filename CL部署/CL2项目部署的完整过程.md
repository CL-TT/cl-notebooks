# 项目部署的完整过程

我们先从两个问题来出发

1. 用户是怎么访问到我们开发好的网站的？

2. 我们的项目开发完毕之后是怎么上线的？



## 1. 网站的运行机制

#### 1.1 名词解释

1. **域名**： 域名俗称网址，用于标识互联网上的计算机，原本标识计算机是用IP地址，但是IP地址不利于记忆，所以人们设计出域名，然后通过DNS将域名和IP地址进行关联，就可以直接通过域名访问到对应的计算机。
2. **DNS服务器**： DNS（Domain Name system）可以理解为互联网上的一项服务，他可以将域名转换成对应的IP地址，可以将其理解为字典，字典中存储的就是域名和IP地址一一对应的键值对。
3. **服务器**： 服务器其实就是一台计算机，跟我们自己平常使用的不一样，它是提供某项互联网服务的。



## 2. 网站请求流程

#### 2.1 前后端分离的页面

1. 前后端分离的项目中，页面中的数据渲染是在浏览器中完成的，分为两个部分，静态页面请求 + ajax数据请求

```js
1. 先在浏览器中输入一个域名

2. 首先会看浏览器的缓存有没有这个域名，如果有的话，就返回其对应的ip地址
   如果没有的话，就看本机的host有没有这个域名(本机的host是没有缓存的)
   如果还没有的话，就看自家的路由器有没有 
   如果还是没有的话，就会向上一层路由器进行寻找，
   如果还是没有，就会向城市的DNS服务器进行寻找，直到在Global这一级的DNS服务器都没找到才算结束
   
3. 找到对应的IP地址之后，会根据这个IP地址去找到对应的服务器

4. 然后Nginx会先到网站根目录去找index.html,通过Nginx把index.html返回给浏览器，浏览器解析，在解析的过程中如果遇到css,js,图片还是这个步骤去请求

5. 数据请求，可以通过Nginx转发， www.abc.top/api =>  Nginx  =>  Nodejs

整个过程如下图
```

![bushu](D:\CL武林秘籍\画图\bushu.png)



#### 2.2 前后端不分离

1. 如果前后端不分离，就不通过Nginx， 当服务器收到浏览器的请求之后，通过NodeJs先到数据库拿到数据，在到网站根目录拿到网页，在NodeJs就把数据渲染到页面上了，所以返回到浏览器的页面，数据已经渲染好了。



## 3. 网站上线部署流程

#### 3.1 购买服务器

1. 我购买的是阿里云服务器



#### 3.2 购买域名

1. 我是在万网购买的域名



#### 3.3 域名解析(配置DNS)

1. 注册好域名之后，需要将域名映射到自己服务器对应的IP地址，注意域名解析会有延迟

2. 解析流程：

   #1. 登录阿里云官网，打开控制台，选择域名，有一个解析，点击解析进入域名解析页面

   #2. 点击添加记录，我添加了三条记录

   | 主机记录 | 记录值        | 含义                                           |
   | -------- | ------------- | ---------------------------------------------- |
   | *        | 121.199.41.46 | *代表泛解析，匹配其他所有的域名，*。*.clei.top |
   | www      | 121.199.41.46 | 解析后的域名为，www.clei.top                   |
   | @        | 121.199.41.46 | 直接解析主域名，clei.top, 同样可以访问         |



#### 3.4 服务器环境搭建

我是window环境，我用的是git bash

```js
1. 先连接服务器  ssh root@域名 => ssh root@www.clei.top
密码是 Caolei1314

2. 安装CentOS开发人员相关包  yum groupinstall 'Development tools'

3. 安装Nginx
# 添加 Nginx 源
sudo yum install epel-release
# 安装 Nginx
sudo yum install nginx
# 启动 Nginx
nginx
# 配置防火墙规则
sudo firewall-cmd --permanent --zone=public --add-service=http 
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload


4. 安装NodeJs
# yum自带源中没有Node.js,所以首先要获取Node.js资源：
curl --silent --location https://rpm.nodesource.com/setup_14.x | bash -
# 安装 Node.js
yum install -y nodejs
# 安装完成之后使用如下指令测试安装是否成功
node -v
# 安装pm2 node.js程序管理工具
npm i pm2 -g
# 使用pm2 启动node.js项目
pm2 start 文件名
# 停止
pm2 stop 文件名或者id
# 从pm2的管理列表中删除
pm2 delete 文件名或者id


5. 安装MySql
# 下载并安装 MySQL 源
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
sudo yum localinstall mysql80-community-release-el7-3.noarch.rpm
# 安装 MySQL
sudo yum install mysql-community-server -y
# 如果上一步报错 执行下面的语句 之后 再次执行一下上面的安装Mysql的语句
sudo yum module disable mysql
# 启动MySQL
sudo systemctl start msyqld
# 找到默认密码
# MySQL安装完毕之后会自动设置一个默认密码，我们需要找到默认密码
grep 'temporary password' /var/log/mysqld.log
# 连接到MySQL数据库，修改密码
mysql -uroot -p
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Caolei_tt1314';
exit;  退出mysql

6. 上传网站资源
#1. scp 本地文件 root@域名:远程路径   =>   scp dist.zip root@www.clei.top:~/
#2. 把网页文件移动到创建好的文件夹里  mv dist.zip /home/nginx
#3. 解压压缩文件 cd /home/nginx   unzip dist.zip
#4. 修改文件名称 mv dist clblog
#5. 删除某个文件 rm 文件名
#6. 删除某个文件夹 rm -r 文件夹的名称   rm -rf  强制删除无需确认


7. 配置Nginx
#1. cd /etc/nginx/conf.d
#2. 创建配置文件   touch clblog.conf
#3. vim clblog.conf
按i键 进出插入模式

插入如下内容
server {
    listen       80;
    server_name  www.clei.top;

    location / {
        root   /home/nginx/clblog;
        index  index.html index.htm;
    }
    
}

保存退出
按一下esc退出编辑模式
然后输入 下面的内容 敲回车
:wq


8. 接口项目部署步骤
#1. scp CLPersonalBlog2.0.zip root@www.clei.top:~/
#2. mv CLPersonalBlog2.0.zip /home/nginx
#3. cd /home/nginx  unzip CLPersonalBlog2.0.zip
#4. 进入到项目目录，安装依赖, 然后启动项目 cd CLPersonalBlog2.0，  npm install，  pm2 start index.js
#5. 修改数据库的密码规则
use mysql;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Caolei_tt1314' PASSWORD EXPIRE NEVER;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Duyi_666duyi';
# 查看是否已经修改成功
select user,host,plugin from user where user='root';
#创建新的数据库
create database my_blog2

#6. 在之前的nginx配置文件中添加反向代理的配置(修改了配置文件就需要重启Nginx)
location ^~ /api/ {
  		rewrite  ^/api/(.*)$ /$1 break;
    	proxy_pass  http://127.0.0.1:9528;
 }
 
后面如果想换后端的包的换

1. 先修改正在使用的包名
   mv CLPersonBlog6.0 CLPersonBlog6.x

2. 把CLPersonBlog6.0中的public下面文件删完
   rm -rf /home/nginx/CLPersonBlog6.0/public/*

3. 把原先的public里面所有内容复制到新的public下面
   cp -r /home/nginx/CLPersonBlog6.1/public/* /home/nginx/CLPersonBlog6.0/public/
```



## 4. 遇到的问题

```js
1. 我在后端用svg-captcha做了一个验证码， /api/verifty 请求验证码的时候，我在后端做了这样的处理

req.session.verifty = value;   我把验证码的值保存到了session中


2. 在做前端登录验证的时候，  /api/admin/login   参数{ loginId: '', loginPwd: '', verifty: value }

传到后端，我会做一个验证码的验证，从之前的req.session.verifty === req.body.verifty

3. 在本地的服务器是没有问题的

4. 但是部署到正式的服务器，就出现了，在登录时， 这个req.session.verifty 为 undefined， 就没有这个字段了

5. nginx配置如下

server {
    listen       80;
    server_name  www.clei.top;

    location / {
        root   /home/nginx/clblog;
        index  index.html index.htm;
    }
    
    location ^~ /api/ {
  		rewrite  ^/api/(.*)$ /$1 break;
    	proxy_pass  http://127.0.0.1:9528;
    }
}


```

