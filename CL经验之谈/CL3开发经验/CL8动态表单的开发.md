# 动态表单的开发

## 1. 确定数据格式

```js
const fieldList = [
    {
        varName: '',    // 这个字段的名称
        controlType: '',  // 组件类型，是输入框还是下拉框
        isRequired: '', // 是否必填
        fieldName: '',  // 如果是字典数据，所需要的中文名称
        maxLen: '',  // 输入类型时，最大长度
        varType: '',    // 字段类型，是字符串还是数字
        dictCode: '',   // 如果是下拉数据，下拉数据的字典值
        label: '',      // label名称
    }
]
```



## 2. 动态表单组件

```vue
<template>
  <el-form
   ref="ruleForm"
   :model="ruleForm"
   :rules="rules"
   :label-width="labelWidth">
      <DynamicForm
        :rule-form="ruleForm"
        :optType="optType"
        :fieldList="[]"
       />
  </el-form>
</template>

<script>
    export default {
        data() {
            return {
                ruleForm: {},
                rules: {},
                labelWidth: ''
            }
        }
    }
</script>
```

```vue
封装的动态表单

<template>
  <div>
    <!--防止el-col错位的-->
    <el-row :type="isFlex ? 'flex' : ''" :style="isFlex? flexStyle : ''">
      <el-col
        :span="8"
        v-for="(item, index) in fieldList"
        :key="index"
      >
        <!-- 文本类型 -->
        <el-form-item
          v-if="item.controlType == 1"
          :label="`${item.label}：`"
          :prop="item.isRequired ? item.varName : ''"
        >
          <span slot="label" v-if="item.tips">
            <WhyIcon :label="item.label" :content="item.tips" />
          </span>
          <el-input
            v-model.trim="ruleForm[item.varName]"
            size="small"
            :maxlength="item.maxLen"
            placeholder=""
            clearable
            :disabled="numOrAreaDis(item)"
            @input="(val) => valueInputFilter(val, item)"
            @blur="(e) => valueBlur(e, item)"
          />
        </el-form-item>

        <!-- 日期控件 -->
        <el-form-item
          v-else-if="item.controlType == 4"
          :label="`${item.label}：`"
          :prop="item.isRequired ? item.varName : ''"
        >
          <span slot="label" v-if="item.tips">
            <WhyIcon :label="item.label" :content="item.tips" />
          </span>
          <el-date-picker
            v-model="ruleForm[item.varName]"
            size="small"
            style="width: 100%"
            type="date"
            value-format="yyyy-MM-dd"
          />
        </el-form-item>

        <!-- 单层下拉组件 -->
        <el-form-item
          v-else-if="item.controlType == 2 && item.sourceType == 'dict'"
          :label="`${item.label}：`"
          :prop="item.isRequired ? item.varName : ''"
        >
          <span slot="label" v-if="item.tips">
            <WhyIcon :label="item.label" :content="item.tips" />
          </span>
          <el-select
            v-model="ruleForm[item.varName]"
            size="small"
            placeholder="请选择"
            @change="(val) => { selectChange(val, item) }">
            <el-option
              v-for="option in item.columnsData"
              :key="option.value"
              :label="option.label"
              :value="option.value"/>
          </el-select>
        </el-form-item>

        <!-- 树形组件 多层下拉和存放地-->
        <el-form-item
          v-else-if="item.controlType == 2 && (item.sourceType == 'dict_tree' || item.sourceType == 'location')"
          :label="`${item.label}：`"
          :prop="item.isRequired ? item.varName : ''"
        >
          <span slot="label" v-if="item.tips">
            <WhyIcon :label="item.label" :content="item.tips" />
          </span>
          <treeselect
            v-model="ruleForm[item.varName]"
            :options="item.columnsData || []"
            :normalizer="normalizer"
            :disable-branch-nodes="item.sourceType == 'dict_tree'"
            class="info-card"
            placeholder="请选择"
            noOptionsText="暂无数据"
            @input="(val) => { treeInput(val, item) }"
            @select="(val) => { treeSelect(val, item) }"
          />
        </el-form-item>

        <!-- 树形组件部门 -->
        <el-form-item
          v-else-if="item.controlType == 2 && (item.sourceType == 'organization')"
          :label="`${item.label}：`"
          :prop="item.isRequired ? item.varName : ''"
        >
          <span slot="label" v-if="item.tips">
            <WhyIcon :label="item.label" :content="item.tips" />
          </span>
          <treeselect
            v-model="ruleForm[item.varName]"
            :options="item.columnsData || []"
            :normalizer="normalizerDep"
            :disable-branch-nodes="false"
            :clearable="ruleForm[item.textField] ? true : false"
            class="info-card"
            placeholder="请选择"
            noOptionsText="暂无数据"
            @input="(val) => { deptInput(val, item) }"
            @select="(val) => { deptSelect(val, item) }"
          >
            <div slot="value-label" slot-scope="{ node }">
              {{ node.raw.cname ? node.raw.cname : null }}
            </div>
          </treeselect>
        </el-form-item>

        <!-- 树形组件人 -->
        <el-form-item
          v-else-if="item.controlType == 2 && (item.sourceType == 'user')"
          :label="`${item.label}：`"
          :prop="item.isRequired ? item.varName : ''"
        >
          <span slot="label" v-if="item.tips">
            <WhyIcon :label="item.label" :content="item.tips" />
          </span>
          <treeselect
            v-model="ruleForm[item.varName]"
            :options="item.columnsData || []"
            :normalizer="normalizerM"
            :disable-branch-nodes="false"
            :clearable="ruleForm[item.textField] ? true : false"
            class="info-card"
            placeholder="请选择"
            noOptionsText="暂无数据"
            @input="(val) => { peopleInput(val, item) }"
            @select="(val) => { peopleSelect(val, item) }"
          >
            <div slot="value-label" slot-scope="{ node }">
              {{ node.raw.label ? node.raw.label : null }}
            </div>
          </treeselect>
        </el-form-item>
      </el-col>
    </el-row>
    <div></div>
  </div>
</template>

```



## 3. 数据操作流程

```js
1. 拿到后端的数据，先处理数据

2. 处理数据可分为对字段的处理，初始化必填规则，初始化字典数据

/**
 * 初始化必填规则
 */
initRules() {
  for(let prop in this.sourceData) {
    this.sourceData[prop].map(item => {
      if (item.isRequired) {
        this.rules[item.varName] = [
          {
            required: true,
            message: item.controlType == 1 ? `请输入${item.label}` : `请选择${item.label}`,
            trigger: item.controlType == 1 ? 'blur' : 'change'
          }
        ]
      }
    })
   }
 }
```

