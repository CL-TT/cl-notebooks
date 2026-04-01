# multer 服务器上传图片

## 1. multer的使用(最佳实践)

上传的 格式**`multipart/form-data`** 

```ts
1. 安装multer

yarn add multer
yarn add uuid
yarn add -D @types/uuid

2. 上传文件存在的问题

#1. 上传的文件存在哪，存储的名称是什么，后缀名取什么
#2. 怎么限制上传文件的格式，大小


3. 封装一个上传文件的中间件

import mluter from 'mluter'
import path from 'path'
import { v4 as uuidv4 } from 'uuid';

// 存储配置：临时存 temp，后续业务再转存正式目录/云OSS
const storage = multer.diskStorage({
   // 把文件存放在哪个位置
  destination: (req, file, cb) => {
    cb(null, path.resolve(__dirname, '/public/upload'));
  },
  // 上传后的文件名称
  filename: (req, file, cb) => {
    // 唯一命名：uuid + 原后缀，防止重名覆盖
    const ext = path.extname(file.originalname);
    const newName = `${uuidv4()}${ext}`;
    cb(null, newName);
  }
});

// 文件过滤：白名单校验（只允许指定后缀/MIME）
const fileFilter = (req, file, cb) => {
  // 图片白名单示例
  const allowExt = ['.jpg', '.jpeg', '.png', '.gif', '.webp'];

  const fileExt = path.extname(file.originalname).toLowerCase();
  
  if (allowExt.includes(fileExt)) {
    cb(null, true);
  } else {
    cb(new Error('仅支持后缀为 jpg/png/gif/webp 图片'), false);
  }
};

// 全局限制：单文件大小、数量
const upload = multer({
  storage,
  fileFilter,
  limits: {
    fileSize: 5 * 1024 * 1024, // 5MB
    files: 5        // 最多同时5个文件
  }
});

export default upload



4. 使用

单个是
app.post('/upload/single', upload.single('avatar'), (req, res) => {});

多个是
app.post('/upload/multi', upload.array('images', 5), (req, res) => {});

// 单个文件上传：表单字段名 avatar
app.post('/upload/single', (req, res) => {
  // 错误处理机制
  upload(req, res, function (err) {
    if (err) {
       res.send({ msg: err.msg })   
    } else {
       // 上传成功
       let path = `/download/${req.filename}`
       res.send({ data: path })
    }
  })
});

```

