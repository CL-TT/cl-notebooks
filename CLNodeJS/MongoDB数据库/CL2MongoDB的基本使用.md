# MongoDB的基本使用

## 1. MongoDB的使用

1. Mongodb默认使用命令下的盘符作为存储盘符，在这个盘符下必须要有 data/db/  文件目录作为自己的数据存储目录。
2. 启动    mongod
3. 连接    mongo
4. 退出    exit   /     ctrl+c

## 2. 常用命令

1. show dbs      查看所有数据库名
2. use 数据库名(如果没有则会创建，但不会马上创建)      相当于创建了数据库
3. db    查看当前操作的数据库
4. show collections    查看所有集合(这个集合相当于表)
5. db.集合名.insertOne({'name':'cl'})                插入一个数据
6. db.集合名.find()    查看数据