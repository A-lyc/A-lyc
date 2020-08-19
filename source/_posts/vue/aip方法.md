---
title: vue-api
date: 2020-08-19 20:07:03
tags: vue
category: vue-api
---

### 组件构造器吧
```js
import Toast from './Toast'
const obj = {}
//全据注册一个组件 obj
obj.install = function(Vue) {
	
//1:创建组件构造器
const toastContrustor = Vue.extend(Toast)
	
//2:根据new的方式，根据组件构造器，可以创建出来一个组件对象
const toast = new toastContrustor()
	
//3:将我们的组件对象手动挂载到一个元素中
toast.$mount(document.createElement('div'))
	
//4:toast.$el对应的就是上面创建的div'
document.body.appendChild(toast.$el)

//暴漏全局为$toast
Vue.prototype.$toast = toast
}
```

```vue
<template>
<div class='toast' v-show="show">
  <div>{{message}}</div>
</div>
</template>

<script>
export default {
name:'Toast',
//import引入的组件需要注入到对象中才能使用
components: {},
data(){
  return {
    message:'',
    show:false
  }
},
methods:{
  isShow(message = '默认文字',duration = 2000){
    this.show = true;
    this.message = message
    setTimeout(()=>{
    this.show = false;
    this.message = ''
    },duration)
  }
}
}
</script>
<style lang='scss' scoped>
//@import url(); 引入公共css类
.toast {
  position:fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  padding: 8px 10px;
  border-radius: 5px;
  font-size: 20px;
  background-color: rgba(0,0,0,.75);
  color: #fff;
  z-index: 999;
}
</style>
```

Js使用，传入methods的方法比如：this.$toast.isShow(‘XX’,2000)
	isShow接收的两个形参
	export default obj

### 注册全局组件
```js
Vue.component( id, [definition] )
// 注册组件，传入一个扩展过的构造器 
Vue.component('my-component', Vue.extend({ /* ... */ })) 
// 注册组件，传入一个选项对象 (自动调用 Vue.extend) 
Vue.component('my-component', { /* ... */ }) 
// 获取注册的组件 (始终返回构造器) 
var MyComponent = Vue.component('my-component')

//使用<my-component />
```

### 监听数据更新
```js
// 修改数据 vm.msg = 'Hello' 
// DOM 还没有更新 
Vue.nextTick(function () { 
// DOM 更新了 
}) 
// 作为一个 Promise 使用 (2.1.0 起新增，详见接下来的提示)
 Vue.nextTick() .then(function () { // DOM 更新了 })
组件内this.$nextTick()
methods: {
 updateMessage: async function () {
 this.message = '已更新' 
console.log(this.$el.textContent) // => '未更新'
 await this.$nextTick() 
console.log(this.$el.textContent) // => '已更新' 
} }

SET写入
Vue.set( target, propertyName/index, value )
```

### 全局指令
```js

// 注册 
Vue.directive('my-directive', { 
bind: function () {}, 
inserted: function () {}, 
update: function () {}, 
componentUpdated: function () {}, 
unbind: function () {} 
}) 
// 注册 (指令函数) 
Vue.directive('my-directive', function () { 
// 这里将会被 `bind` 和 `update` 调用
 }) 
// getter，返回已注册的指令 
var myDirective = Vue.directive('my-directive')

//<view @my-directive='as'>
```

### 过滤
// 注册 
```js
Vue.filter('my-filter', function (value) { 
// 返回处理后的值
 }) 
 // getter，返回已注册的过滤器 
 var myFilter = Vue.filter('my-filter')
```