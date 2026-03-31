# 导入导出的几种方式

## 1. 应用背景

1. 在很多的后台管理系统中，可能会有导入，导出操作， 下面是解决方案。



## 2. 解决方案

### 1. 第一种方案

1. 前端请求后端的接口，后端返回文件路径，前端直接下载

```js
1. window.location.href = '/api/xxxxxx'

这种方式非常方便，但缺点也很明显，不能修改文件的名称


2. window.open('/api/xxxxxx')
```



### 2. 第二种方案

1. 这种方式就是正常的api请求，后端以文件流的形式发送给前端，前端获取到文件数据之后，在本地模拟一次点击按钮下载， 这次下载是把后端返回的文件数据转换成合适的格式之后下载下来

```js
export const exportFile = () => {
    return request({
        url: '',
        method: '',
        headers: {},
        responseType: 'blob',
    })
}

//以excel文件作为例子

exportFile().then(res => {
    if(res.status === 200){
        const xlsx = 'application/vnd.ms-excel'
        const blob = new Blob([res.data], { type: xlsx })
        const a = document.createElement('a')
        a.download = '文件名称'
        a.href = window.URL.createObjectURL(blob)
        a.click()
        a.remove()
    }
})
```



## 3. 使用post方法的导出

```js
/**
* obj 要查询的参数
**/
openPost(obj = {}) {
      const tempForm = document.createElement('form') // 创建一个临时的表单元素
      tempForm.id = 'tempForm'
      tempForm.method = 'post'
      tempForm.target = '_blank'
      tempForm.action = `api?access_token=${localStorage.getItem('token')}` // 接口地址
      const hideInputArr = []
      for(const prop in obj) {
          const hideInput = document.createElement('input')
          hideInput.type = 'hidden'
          hideInput.name = prop
          hideInput.value = obj[prop]
          tempForm.appendChild(hideInput) // 把每一个input添加到表单中
      }
      tempForm.addEventListener('onsubmit', () => {
        window.open('about:blank')
      })
      document.body.appendChild(tempForm)
      tempForm.submit()
      document.body.removeChild(tempForm)
}
```

```js
优化过后的代码

/**
 * params: { name: '', age: 20 }
 **/
function openPostWindow(params){
    var tempForm = document.createElement("form");
    tempForm.id = "tempForm1";
    tempForm.method = "post";
    tempForm.action = `/api/inventory/labelManual/labelExport`;
    tempForm.target="_blank"; //打开新页面
    tempForm.acceptCharset = 'application/json;charset=UTF-8'

    for(var key in params){
      tempForm.appendChild(this.generateInput(key,params));
    }
    
    if (document.all) {
      tempForm.attachEvent("onsubmit",function(){});//IE
    } else {
      var subObj = tempForm.addEventListener("submit",function(){},false);//firefox
    }
    document.body.appendChild(tempForm);
    if (document.all) {
      tempForm.fireEvent("onsubmit");
    } else {
      tempForm.dispatchEvent(new Event("submit"));
    }
    tempForm.submit();
    document.body.removeChild(tempForm);
  },
      
  generateInput(key,params){
    var hideInput = document.createElement("input");
    hideInput.type = "hidden";
    hideInput.name = key;
    hideInput.value = params[key];
    return hideInput;
  },
```

