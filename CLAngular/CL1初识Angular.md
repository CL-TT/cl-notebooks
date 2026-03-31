# 初识Angular

## **1. Angular的版本问题**



1. Angular 1.0  => AngularJs   这是一类版本



2. Angular 2.0 以后的版本， Angular 4.0 5.0 是一类版本,  兼容了TS



3. 这两类版本互不兼容，是两个完全不同的内容



4. 为什么没有Angular3.0?

   因为在Angular2.0版本时，它的路由模块是3.0版本，为了让版本号对齐，所以就跳过了Angular3.0版本



## **2. 全局安装CLI**

npm install -g @angular/cli



## 3. 创建项目

ng new my-app

npm install 

ng serve --open  打开项目



## 4. **Angular项目目录结构**



\1. polyfills.ts => 让不兼容的浏览器去兼容它



\2. NgModule是一个装饰器函数，它接受一个用来描述模块属性的元数据对象， 它有几个属性



@1. declarations: 声明本模块中拥有的视图类， 有三种视图类 组件， 指令， 管道



@2. exports: declarations的子集，可用于其他模块的组件模板



@3. imports: 本模块声明的组件模板需要的类所在的其他模板



@4. providers: 服务的创建者，并加入到全局服务列表中，可应用于任何部分



@5. bootstrap: 制定应用的主视图(称为根组件) 他是所有其他视图的宿主，只有根模块才能设置bootstrap属性