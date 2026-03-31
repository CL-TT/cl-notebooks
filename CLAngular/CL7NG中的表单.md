# NG中的表单

## 1. 模板驱动表单校验

1. ngModel指令不仅仅跟踪状态， 他还使用特定的css类来更新控件，以反映当前的状态

2. | 状态           | 为真时的css类名 | 为假时的css类名 |
   | -------------- | --------------- | --------------- |
   | 控件被访问过了 | ng-touched      | ng-untouched    |
   | 控件的值变化了 | ng-dirty        | ng-pristine     |
   | 控件的值有效了 | ng-valid        | ng-invalid      |

   

```html
1. #name 单独的这个指的是dom元素
2. #name="ngModel" 指的是这个ngModel这个变量所附带的东西


<form (ngSubmit)="handleSubmit()">
 <input type="text" [(ngModel)]="form.name" name="name" required minlength="5" #name="ngModel">
 <button type="submit" [disabled]="!name.valid">提交</button>
</form>

<script>
    form = {
        name: ''
    }
    handleSubmit(){
        consoel.log(form)
    }
</script>
```



## 2. 响应式表单

### 1.使用步骤

1. 第一步 在app.module中引入  import { ReactiveFormsModule } from '@angular/forms';

```html
2. 第二步 使用form表单， 表单上绑定属性 [formGroup]="formGroup"
<form [formGroup]="formGroup" (onSubmit)="handleSubmit">
    <input type="text" name="name" formControlName="name">
    <input type="password" name="password" formControlName="password">
</form>

<script>
    import { FormGroup, FormControl, Validators } from '@angular/forms'
    
    form = {
        name: '',
        password: ''
    }
    
    formGroup: FormGroup = new FormGroup({})
    
    ngOnInit(){
        // 代表整个表单
        this.formGroup = new FormGroup({
            "name": new FormControl(this.form.name, [
                Validators.required,
                Validators.minlength(4)
            ])
        })
    }
    
    /**
    * 可以通过this.formGroup来获取表单的值以及验证结果
    * valid: 验证结果
    * value: 表单的值 { name: '', password: '' }
    **/
    handleSubmit(){
        if(!this.formGroup.valid) return 
        console.log(this.formGroup.value)
    }
</script>
```



## 3. formBuilder的使用

#### 1. 为什么会出现formBuilder

1. 因为响应式表单的校验是由formGroup, formControl混合使用组成的， 这种写法比较繁琐
2. 所以出现formBuilder来简化验证

```html
<form (ngSubmit)="handleSubmit()" [formGroup]="formGroup">
  <input type="text" name="name" formControlName="name">
  <input type="password" name="password" formControlName="password">
  <button type="submit">提交</button>
</form>

<script>
    import { FormBuilder, FormGroup, Validators } from '@/angular/form'
    
    formGroup: FormGroup;
    
    constructor(fb: FormBuilder){
        thia.formGroup = fb.group({
            // 第一个是默认值， 第二个数组是验证规则
            name: ['', [Validators.required, Validators.minLength(4)]],
            password: ['', []]
        })
    }
</script>
```



#### 2. 重置表单

```html
this.formGroup.reset();
```



#### 3. 设置表单的值

1. setValue,   传递值必须是表单值的全量， 会触发表单提交事件
2. patchValue, 传递的值可以是表单的一个或者几个值， 同样会触发表单提交事件

```html
// 调用setValue方法会触发提交表单的方法

this.formGroup.setValue({
  name: 'niahaj',
  password: 'jssksksks'
})

this.formGroup.patchValue({
  name: 'nsjs'
})
```



#### 4. 嵌套表单的使用

```html
<form (ngSubmit)="handleSubmit()" [formGroup]="formGroup">
  <input type="text" name="name" formControlName="name">
  <input type="password" name="password" formControlName="password">
  <div formGroupName="address">
      <input type="text" formControlName="street">
  </div>
  <button type="submit">提交</button>
</form>

<script>
    import { FormBuilder, FormGroup, Validators } from '@/angular/form'
    
    formGroup: FormGroup;
    
    constructor(fb: FormBuilder){
        thia.formGroup = fb.group({
            // 第一个是默认值， 第二个数组是验证规则
            name: ['', [Validators.required, Validators.minLength(4)]],
            password: ['', []],
            // 嵌套表单
            address: fb.group({
                street: ['街道']
            })
        })
    }
    
    this.formGroup.value => { name: '', password: '', address: { street: '' } }
</script>
```

