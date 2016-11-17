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
// 配置全局form表单序列化之前的hook
hookForm.beforeSerialize = next => {
  // next：继续执行
  // 你可以在此处执行表单校验
}
// 配置全局form提交前的hook
hookForm.onSubmit = request => {
  // request：包含了本次请求的基本信息
  // 你可以在此处执行表单校验或使用ajax来提交本次请求
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
  <hook-form action="users" :on-submit="onSubmit" :before-serialize="beforeSerialize">
    <input name="name">
    <input name="age">
    <button>提交</button>
  </hook-form>
</template>
<script>
  export default {
    methods: {
      // 局部hook
      beforeSerialize (next) {
        // next：继续执行
        // 你可以在此处执行表单校验
      },
      onSubmit (request) {
        // request：包含了本次请求的基本信息
        // 你可以在此处执行表单校验或使用ajax来提交本次请求
      }
    }
  }
</script>
```
## 配置
### onSubmit
form提交前的全局hook。参见上文**注册&配置组件**

### beforeSerialize
form表单序列化之前的全局hook。参见上文**注册&配置组件**<br>
一般用于校验表单，但此时无须form表单序列化的时候。也便于在触发onSubmit之前更改form表单内容

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
* onSubmit：form提交前的局部hook
* beforeSerialize: form表单序列化之前的局部hook
* json：Request.body是否转成json格式（优先使用局部json配置）。默认：false
* disabled：禁用

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
## 常见问题
### 全局onSubmit中提交了请求，返回了请求。这些操作如何通知到对应的组件？
Request对象中有一个hookFormVm属性，这是hookForm组件本身的vue实例。[父子组件通信](http://cn.vuejs.org/guide/components.html#u7236_u5B50_u7EC4_u4EF6_u901A_u4FE1) <br>
也就是说hookForm只是扮演form和ajax之间的桥梁，负责传送一下form表单数据。你可以基于它二次封装一个更多功能的form组件

### 全局hook和局部hook会不会同时触发？
不会。如果存在局部hook，那么优先触发局部，否则才触发全局。

