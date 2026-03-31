# 文件流的操作

## 1.流的认识

数据从一个地方缓慢的流入到另一个地方，而不是一次性的全部过来

流的方向：  可读流  磁盘到内存   ||    可写流    内存到磁盘     ||      双工流





## 2.文件流的操作演示

```javascript
//1. 先通过读取流，读取文件内容
//2. 在通过写入流把文件内容写入， 如果写入队列已经满了，那么久暂停文件的读取
//3. 等到队列清空时， 就可以恢复文件的读取

const fs = require('fs');
const rs = fs.createReadStream(filepath, {encoding: 'utf-8', highWaterMark: 100});
const ws = fs.createWriteStream(filepath, {encoding: 'utf-8'}, highWaterMark: 100);
function method(){
    //通过监听data事件来读取文件内容
    rs.on('data', chunk => {
        //边读边写
        const flag = ws.write(chunk);
        
        //看是否出现背压问题
        if(!flag){
            //出现背压问题，那么就暂停读取
            rs.pause();
        }
    });
    
    //监听写入队列是否清空
    ws.on('drain', () => {
        rs.resume();   //恢复读取
    })
}
```



## 3.具体流程

![文件流](C:\Users\caolei\Pictures\笔记图片\文件流.png)

