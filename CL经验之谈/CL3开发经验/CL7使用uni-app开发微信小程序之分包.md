# uni-app开发微信小程序之分包

## 1. 步骤

1. 在manifest.json文件中进行配置

   ```js
   "mp-weixin": {
       "optimization": {
           "subPackages": true
       }
   }
   ```

   

2. 配置pages.json

   ```js
   在pages.json中新建数组subPackages, 数组中包含两个参数， root, pages
   
   {
       "pages": [], // 主包
           
       "subpackages": [
           {
             "root": "pagesA",
             "pages": [{
                 "path": 'list/list'   // 这个路径不用加pagesA
             }]
           },
           {
             "root": "pagesB",
             "pages": [{
                 "path": 'list/list'   // 这个路径不用加pagesB
             }]
           }
       ]
   }
   ```



3. 分包的包文件夹和主包的文件夹在同一等级
4. 不会影响打包成app