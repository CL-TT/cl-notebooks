# 快速搭建服务器之json-server的使用

## 1. json-server的使用

```react
1. npm i json-server

2. 在目录下面创建一个json文件   db.json

属性名就是接口名称

{
  "stulist": [
    {
      "name": "张三",
      "age": 20
    },
    {
      "name": "李四",
      "age": 24
    },
    {
      "name": "王五",
      "age": 29
    },
    {
      "name": "麻溜",
      "age": 38
    }
  ]
}

3. 在package.json中添加配置

"scripts": {
    "json-server": "json-server --watch db.json"
 }


4.  运行npm run json-server命令
```



## 2. 请求的用法

```react
{
    "list": [
        {
            "id": 1
            "name": "caolei"
        },
        {
            id: 2,
            "name": "张三"
        }
    ]
}

1. 过滤获取

http://localhost:3000/list/1,    可以指定到id为1的数据

http://localhost:3000/list?id=1


2. 分页获取

分别采用_page来设置页码，_limit来控制每页显示条数， 如果没有指定，默认每页显示10条

http://localhost:3000/list?_page=1&_limit=5  


3. 排序

排序一般采用_sort来指定要排序的字段，_order来指定排序是正排序还是逆排序 (asc | desc) 默认是asc

http://localhost:3000/list?_sort=age&_order=desc


4. 添加数据

http://localhost:3000/list    post方法，  传一个data参数
```

