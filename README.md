# vue-hook-form
用于处理form请求，获取formData。以便于将form请求转成ajax/fetch请求

## 安装
```
npm install vue-hook-form
```
## 使用
**VUE版本：1.x** <br>
**必须在vue-cli生成的webpack模板环境中使用**
### 一，注册&配置组件
```
// 这里使用在main.js中全局注册来示例
import vue from 'vue'
import hookForm from 'vue-hook-form'
// 配置全局form提交前的hook
hookForm.onSubmit = request => {
  // request：包含了本次请求的基本信息
  // 你可以在此处执行表单校验，然后使用ajax来提交本次请求
}
vue.component('hook-form', hookForm)
```
Request对象：
```
{
  url: '请求地址',
  body: '请求参数',
  method: '请求方法',
  hookFormVm: '对应的hookForm vm实例'
}
```
### 二，vue文件中使用
```
<template>
  <hook-form action="users" :on-submit="onSubmit">
    <input name="name">
    <input name="age">
    <button>提交</button>
  </hook-form>
</template>
<script>
  export default {
    methods: {
      // 局部hook
      onSubmit (request) {
        // request：包含了本次请求的基本信息
        // 你可以在此处执行表单校验，然后使用ajax来提交本次请求
      }
    }
  }
</script>
```
## 属性
### onSubmit
全局form提交前的hook。参见上文**注册&配置组件**

### json
Request.body是否转成json格式，默认为url字符串。默认：false
```
hookForm.onSubmit = request => {
  // request.body为url字符串格式
  // 形如：name=张三&age=18
}
// 配置Request.body为json格式
hookForm.json = true
hookForm.onSubmit = request => {
  // request.body为json格式
}
```
## Props
* action：请求地址
* method：请求方法。默认：post
* onSubmit：局部form提交前的hook
* unhook：禁用全局form提交前的hook
* json：Request.body是否转成json格式（优先使用局部json配置）。默认：false
* disabled：禁止form提交

## 关于unhook
如果配置了全局onSubmit，组件中也配置了onSubmit，默认两个hook都会触发。通过使用该选项可以使本次请求不走全局hook
```
<!--不需要这么写：:unhook="true"-->
<hook-form action="users" :on-submit="onSubmit" unhook>
    <input name="name">
    <input name="age">
    <button>提交</button>
</hook-form>
```
## 关于disabled
你可以通过使用该选项来防止form的多次提交
```
hookForm.onSubmit = request => {
  // 禁止form提交
  request.hookFormVm.disabled = true
  // 在ajax请求或其他相关操作之后再释放禁用。
  doSomeThing()
    .then(() => {
      request.hookFormVm.disabled = false
    })
}
```
