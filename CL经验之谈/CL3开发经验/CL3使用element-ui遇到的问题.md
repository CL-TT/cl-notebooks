# 使用element-ui遇到的问题

## 1. el-input只想使其输入数字

```js
<el-input v-model="ruleForm.sort" @input="sortInput" />
    
    
sortInput(value){
    this.ruleForm.sort = value.replace(/[^\d]/g,'')
}
```



## 2. el-table树形数据让具体的某一列有展开图标

```vue
<el-table>
    <el-table-column label="姓名" type=""></el-table-column>
    <el-table-column label="年龄"></el-table-column>
</el-table>

1. 如果你想要设置第二列或者更后面的列有展开符号, 只需要在那一列的前面设置type=""

2. 设置展开符号的样式 

.el-icon-arrow-right{
}
```



## 3. 动态表单加上动态的验证规则

```vue

```



## 4. 在做什么切换操作时，导致el-table表格错乱

```vue
1. 给表格加key值

2. this.$nextTick(() => {
     this.$refs.table.doLayout();
   })
```



## 5. el-table某些列设置了fixed，横向滚动条滚动不了

```vue
// 这个样式是解决当固定前几列横向滚动条不能移动的问题
/deep/ .el-table__fixed{
  height: auto !important;
  bottom: 16px !important;
}
```



## 6. el-table固定操作列后，出现竖向滚动条，会出现错位情况

```
// 处理el-table有固定列时，出现错位情况

.el-table__body, .el-table__footer, .el-table__header {
    table-layout: fixed !important;
    border-collapse: separate !important;
}
```



## 7. el-table固定操作列，最后一个字段缺失右侧边框

```vue
//表头
thead th:not(.is-hidden):last-child {
  right: -1px !important;
}
//表单
.el-table__row {
  td:not(.is-hidden):last-child {
    right: -1px !important;
  }
}
```



## 8. el-loading中的text文字换行问题

```js
const loading = this.$loading({
   lock: true,
   text: `初始化中\n哈哈哈`,
   spinner: 'el-icon-loading',
   customClass: 'el-loading-spe',
   background: 'rgba(0, 0, 0, 0.6)'
});

.el-loading-spinner{
    white-space: pre-wrap;
}
```



## 9. el-col在el-row中错位的问题

```js
问题：将el-col全放在el-row中，可能会出现分辨率或者缩放，内容不同导致el-col高度不一致，从而使布局异常


解决办法：el-row添加flex布局

<el-row type="flex" style="flex-wrap: wrap" />
```
