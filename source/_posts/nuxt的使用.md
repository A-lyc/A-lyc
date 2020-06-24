---
uuid: 34d68fb9-97e7-6b64-3b66-8737dc36a179
title: nuxt的使用
date: 2020-06-12 10:58:16
tags: nuxt的使用
category: nuxt的使用-vue实现seo
---
##  安装sass
npm install node-sass sass-loader --save-dev
安装之后需要配置nuxt.config.js
```js
/**
 * 新加导入scss
 */
  styleResources: {
    scss: '@/assets/style/global.scss'
  },
   /*
  ** Nuxt.js modules
  */
   modules: [
    // Doc: https://axios.nuxtjs.org/usage
    '@nuxtjs/axios',
    '@gauseen/nuxt-proxy',
    '@nuxtjs/style-resources'//需要npm安装 npm i @nuxtjs/style-resources --save-dev
  ],
```
使用：
@import '@/assets/style/global.scss';
找到进行引入

##  使用axios
需要安装axios，
npm i axios --save-dev
使用axios： - > 参考：vue的axios封装好的这个文件，点击搜索即可需要将baseURL的值修改成跨域之后的值，
```js
const instance = axios.create({
    baseURL: '/api',
    timeout: 1000,
    headers: config.headers || ''
  })

```
##  解决跨域问题
在nuxt.config.js中找到对应的建进行修改

```js
 modules: [
    // Doc: https://axios.nuxtjs.org/usage
    '@nuxtjs/axios',
    '@gauseen/nuxt-proxy',
  ],
  /*
  ** Axios module configuration
  ** See https://axios.nuxtjs.org/options
  */
  axios: {
    // prefix: '/api/',
    //是否允许跨域
    proxy: true,
    // 最多重发三次
    retry: { retries: 3 },
    //开发模式下开启debug
    debug: process.env._ENV == "production" ? false : true,
    //设置不同环境的请求地址
    baseURL:
      process.env._ENV == "production"
        ? "http://localhost:3000/api"
        : "http://localhost:3000/api",
        //是否是可信任
    withCredentials: true
  },
  // 代理服务器 => 本身没有需要自己添加
proxyTable: {
  "/api": {
    target: "https://www.baidu.com",  // 线上地址
    // target: "http://192.168.1.106:7000",  // 本地环境
    pathRewrite: {"^/api" : ""}
  }
},
```
##  路由问题
多个路由传参使用_.vue
```
pages/
--| login/
-----| _.vue
-----| _log
-------|_.vue
--| index.vue
```
可以在:to="login/1/2/3",如果有子元素的话 :to="login/1/log/1" 传输多个参数文件夹_这个符号可以动态传参
假设pages的目录结构如下
```
pages/
--| user/
-----| index.vue
-----| one.vue
--| index.vue
```
那么，Nuxt.js自动生成的路由配置如下
```js
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'user',
      path: '/user',
      component: 'pages/user/index.vue'
    },
    {
      name: 'user-one',
      path: '/user/one',
      component: 'pages/user/one.vue'
    }
  ]
}
```
### 动态路由
在Nuxt.js里面定义带参数的动态路由，需要创建对应的以下划线作为前缀的Vue文件或目录。
以下目录结构
```
pages/
--| _slug/
-----| comments.vue
-----| index.vue
--| users/
-----| _id.vue
--| index.vue
```
Nuxt.js生成对应的路由配置表为
```js
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'users-id',
      path: '/users/:id?',
      component: 'pages/users/_id.vue'
    },
    {
      name: 'slug',
      path: '/:slug',
      component: 'pages/_slug/index.vue'
    },
    {
      name: 'slug-comments',
      path: '/:slug/comments',
      component: 'pages/_slug/comments.vue'
    }
  ]
}
```
你会发现名称为users-id的路由路径带有:id？参数，表示该路由是可选的。原理跟vue动态路由是一致的。

```html
<nuxt-link :to="'/users/' + id?name='四叶草'">首页</nuxt-link>
```

## vuex使用
模块式导出
```js
export const state = () => ({
  counter: 0
})

export const mutations = {
  increment (state,lod) {
    state.counter = lod
  }
}

```

使用：
```js
   add() {
      this.$store.commit('increment', { name: '四叶草' })
      console.log(this.$store.state.counter)
    }
```
##  用法
和vue没有什么区别 安装脚手架的时候可以直接安装element，可直接使用


##  获取win
if (process.browser) {
  //逻辑
}