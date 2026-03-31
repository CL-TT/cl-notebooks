# npm命令

## 1. 安装

​     1. 精确安装最新版本

​         npm install --save-exact 包名

​         npm install -E 包名

​     2. 安装指定版本

​         npm install 包名@版本号

## 2. 查询

​    1. 查询包安装路径

​       npm root [-g]

​    2. 查看包信息

​       npm view 包名 [子信息]

​       view aliases：v info show  别名

​    3. 查询安装包

​       npm list [-g] [--depth=依赖深度]

​       list aliases: ls  la  ll

## 3. 更新

​    npm install -g npm

​    1.检查有哪些包需要更新

​       npm outdated

​    2. 更新包

​        npm update [-g] [包名]

​       update 别名（aliases）：up、upgrade

## 4. 卸载

​      npm uninstall [-g] 包名

​      uninstall aliases: remove, rm, r, un, unlink



## 5. npm配置

​    1. 通过下面的命令可以查询目前生效的各种配置

​      npm config ls [-l] [--json]

​    2. 获取某个配置项

​      npm config get 配置项

​    3. 设置某个配置项

​      npm config set 配置项=值

​    4. 移除某个配置项

​      npm config delete 配置项