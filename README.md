# vue-ajax-form
用于将form提交转成ajax(fetch)请求

## 安装
```
npm install vue-ajax-form
```
## 使用
**VUE版本：1.x** <br>
**必须在vue-cli生成的webpack模板环境中使用**
### 一，注册组件
```
// 这里使用在main.js中全局注册来示例
import vue from 'vue'
import ajaxForm from 'vue-ajax-form'
// 配置全局form提交前的hook
ajaxForm.beforeSubmit = function (request, next) {}
vue.component('ajax-form', ajaxForm)
```
### 二，vue文件中使用
```
<template>

</template>
<script>
  export default {
  }
</script>
```
未完待续。。。
