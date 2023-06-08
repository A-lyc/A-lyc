---
uuid: 81fcd500-a4d5-a890-0cf0-46fd315d66f1
title: vite
date: 2023-06-08 14:23:11
tags: vue vite.config vite
category: vite vue
---
## vue main.js
```js

import { createApp } from 'vue'
import './style.css'
import App from './App.vue'

const app = createApp(App)
// 更改全局配置
// 1. 错误异常处理
app.config.errorHandler = (err,vm,info)=>{
  console.error(err,vm,info)
}
app.config.warnHandler = (err,vm,info)=>{
  console.warn(err,vm,info)
}
// 2. 全局属性设置
app.config.globalProperties.$http = null
// 3. 编译配置项
app.config.compilerOptions.whitespace = "preserve"

// 插件安装
// app.use()


app.mount('#app')




```


## 安装路由
- npm install vue-router --save
### main.js
```js
import router from './router/index.js'
// 路由导航首位
import './router/permission.js' 
app.use(router)
```
### router/index
```js
import * as VueRouter from 'vue-router'
/*
* 路由配置文件，提供几个测试页面; 通过import * 或者懒加载导入()=>import
* 懒加载有助于代码分割，减少包的体积。
* */
const Home = ()=>import('@/views/home.vue')
const list = ()=>import('@/views/list.vue')
const layout = ()=>import('@/layout/layout.vue')
const routes = [
  {
    path:'/',
    component: layout,
    name: 'index',
    children:[
      {
        path: '/index',
        name: 'Home',
        component: Home,
        beforeEnter: (to, from) => {
         console.log('路由独享')
        },
        meta: { title: '个人中心', icon: 'renshu',login:true,side:'个人中心',isShow:true,isShowPC:false  },
      },
      {
        path: '/list',
        name: 'list',
        component: list,
        beforeEnter: (to, from) => {
         console.log('路由独享 Home2')
        },
        meta: { title: '个人中心', icon: 'renshu',login:true,side:'个人中心',isShow:true,isShowPC:false  },
      }
    ]
  }
]

/**
 * 创建router实例
 *
 */
export default VueRouter.createRouter({
  history:VueRouter.createWebHistory(),
  routes
})
```
### permission
```js
import router from './index.js'

//你可以使用 router.beforeEach 注册一个全局前置守卫：
router.beforeEach((to, from) => {
  // ...
  // 返回 false 以取消导航
  console.log('你可以使用 router.beforeEach 注册一个全局前置守卫：')
})
// 你可以用 router.beforeResolve 注册一个全局守卫
router.beforeResolve(async to => {
  console.log(to)
  console.log('注册一个全局守卫')
})
router.afterEach((to, from) => {
  console.log('全局后置钩子')
})
```

## 安装pinia == vuex
- npm i pinia
### main 
```js
import { createPinia } from 'pinia'
const pinia = createPinia()
app.use(pinia)
```
### store/index
```js
import { defineStore } from 'pinia'
//这个名字 ，也被用作 id ，是必须传入的， Pinia 将用它来连接 store 和 devtools。为了养成习惯性的用法，将返回的函数命名为 use... 是一个符合组合式函数风格的约定。
export const useAlertsStore = defineStore('alerts', {
  state: () => ({ count: 0 }),
  getters: {
    double: (state) => state.count * 2,
  },
  actions: {
    increment() {
      this.count++
    },
  },
})
```

也存在另一种定义 store 的可用语法。与 Vue 组合式 API 的 setup 函数 相似，我们可以传入一个函数，该函数定义了一些响应式属性和方法，并且返回一个带有我们想暴露出去的属性和方法的对象。
```js
export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  function increment() {
    count.value++
  }
  let onclick = computed(()=>{
    return count.value + 'asdasd'
  })
  const firstName = ref('John')
  const lastName = ref('Doe')
  const fullName = computed({
    // getter
    get() {
      return firstName.value + ' ' + lastName.value
    },
    // setter
    set(newValue) {
      // 注意：我们这里使用的是解构赋值语法
      [firstName.value, lastName.value] = newValue.split(' ')
    }
  })
  return { count, increment,onclick,fullName }
})
```

### 使用
需要看文档这个没有搞懂怎么弄
```js
import { useAlertsStore } from '../stoer/counter'
const counter = useAlertsStore()
console.log(counter)
counter.count++
// 自动补全！ ✨
// counter.$patch({ count: counter.count + 1 })
// 或使用 action 代替
counter.increment()
```
