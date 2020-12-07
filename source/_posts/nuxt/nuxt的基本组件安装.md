---
uuid: 34d68fb9-97e7-6b64-3b66-8737dc36a179
title: nuxt的基本组件安装
date: 2099-1-1 10:58:16
tags: nuxt的使用 nuxt的基本组件安装
category: Nuxt
---
------
中途的bug 一定熟练使用element有很大的帮助
<!-- more -->
0：上传服务器运行 - 服务器有缓存问题，把自身的缓存关闭，之后开启服务端缓存，期间有很多bug
1：状态存储到vuex中会使网页分享的时候带有用户的token，所以这个token需要在浏览器中获取之后发送网络请求，不要在vuex直接获取发送请求，会有bug
2：vuex有的时候进入页面点击刷新的时候很多东西没有了，因为存储的状态管理没有了，所以导致显示或者不显示的问题，等等，如果用户的话，这个还是判断浏览器中的token有没有，在使用浏览器的token获取用户信息，之后在进行判断，以防刷新之后不显示问题
3：页面数据加载不出来：因为没有正确使用asyncData数据请求，或者axios写错了位置，应该在插件目录下
4：vuex中nuxtServerInit的使用， mutations内使用大写TOKEN(){},之操作一件事情
5：nuxt使用node_mode中的axios的时候会报错官方解释是，所以尽量使用自己带的axios
6：上传之前需要把debug: process.env._ENV == 'production' ? false : true, 关闭注释掉这个是检查debug模式
如果您的项目中直接使用了node_modules中的axios，并且使用axios.interceptors添加拦截器对请求或响应数据进行了处理，确保使用 axios.create创建实例后再使用。否则多次刷新页面请求服务器，服务端渲染会重复添加拦截器，导致数据处理错误。
------
### 自己初始化的nuxt的模板文件
<a href="./template.zip" target="_blank"> template自己定义模板 </a>

####    1：安装
npx create-nuxt-app <project-name>
cd <project-name>
进入环境命令：
npm run dev
打包编译
npm run dev
之后生成 dist（vue）和.nuxt（服务器:如果用中间件需要把中间件的文件一起打包发给后端，基本文件：.nuxt,state,nuxt.confige.js,package.json）
npm i 之后 npm start - 项目可以跑起来了

首先新建项目安装完成npm和node，使用脚手架的命令去新建一个初始化的项目，我基本使用的是npm进行安装，输入命令行就可
npx create-nuxt-app <项目名>
选择默认初始化的项目名称，比如选择UI框架，安装axios，选择渲染方式，之后下一步下一步即可
npm run dev即可进入开发环境
页面内网络请求获取的数据使用asyncData获取，下面会有介绍
点击动作请求数据可使用插件导出的网络请求，下面有介绍 this.$api.getInfo()
使用cscc选要提前安装 npm install --save-dev node-sass sass-loader

<!-- more -->

### 配置nuxt.config.js
```js
export default {
  mode: 'universal',
  /*
  * 头部显示区域 - 页面组件内可以使用
  *  head(){return{title:'',
  *  meta:[{hid: '网站描述',name: 'description',content:''}]}}
  *  来重定义头部标签
  *  
  */
  head: {
    title: '头部标题' || '',//
    meta: [
      { charset: 'utf-8' },
      {
        content: `viewport, width=device-width, initial-scale=1, 
minimum-scale=1.0,maximum-scale=1.0,user-scalable=no`
      },

    ],
    link: [//使用到的css可以使用link标签引入
      { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' },
      { href: 'https://static.com/css/index.css', rel: 'stylesheet' },
      { href: 'https://static.com/css/bootstrap.min.css', rel: 'stylesheet' },
      { href: 'https://static.com/css/bootstrap-vue.min.css', rel: 'stylesheet' }
    ]
  },
  /*
  ** loading的颜色
  */
  loading: { color: '#000' },

  /**
   * 新加导入scss - 需要提前安装 npm install --save-dev node-sass sass-loader
   */
  styleResources: {
    scss: '@/assets/style/global.scss'
  },

  /*
  **  外部导入的css可以直接在这显示，查看源码的时候会直接显示到页面上的
  */
  css: [
    // 'element-ui/lib/theme-chalk/index.css'
    // '@/assets/css/main.scss'
  ],

  /*
  ** 插件配置目录，安装的插件都需要在这个位置配置，有的插件需要获取dom后者win
  ** 建议安装插件的时候全局安装 { src: '@/plugins/map', ssr: false },可以获取dom后者win
  ** 这样即可使用的时候之间按照官网的文档标签使用即可
  ** 插件不单独使用的是因为需要获取dom和win的原因
  ** '@/plugins/api'  api是封装的网络请求，下面会解释到的
  */
  plugins: [
    '@/plugins/element-ui',//element
    '@/plugins/api',//封装的axios
    '@/plugins/bootstrap-vue',//栅格
    '@/plugins/vue-moment',//格式化时间
    { src: '@/plugins/map', ssr: false },//地图
    { src: '@/plugins/vue-quill-editor', ssr: false },//富文本
    { src: '@/plugins/vue-cropper', ssr: false },//裁切
    { src: '@/plugins/md5', ssr: false }//md5加密
  ],

  /*
  ** Nuxt.js dev-modules
  */
  buildModules: [],

  /*
  ** Nuxt.js modules - 请求需要
  */
  modules: [
    // Doc: https://axios.nuxtjs.org/usage
//没有的话都需要安装
    '@nuxtjs/axios',
    '@gauseen/nuxt-proxy',
    '@nuxtjs/style-resources',
    //缓存
    // '@nuxtjs/component-cache',
  ],

  /*
  ** Axios 配置跨域的问题，外部使用 /api/
  ** See https://axios.nuxtjs.org/options
  */
  axios: {
    prefix: '/api/',
    //是否允许跨域
    proxy: true,
    // 最多重发三次
    retry: { retries: 5 },
    //开发模式下开启debug
    debug: process.env._ENV == 'production' ? false : true,
    //设置不同环境的请求地址
    baseURL:
      process.env._ENV == 'production'
        ? 'http://localhost:3000/api'
        : 'http://localhost:3000/api',
    //是否是可信任
    withCredentials: true
  },
//切换网页url不正确 - 具体是意思不明
  publicRuntimeConfig: {
    axios: {
      browserBaseURL: process.env.BROWSER_BASE_URL
    }
  },
//切换网页url不正确 - 具体是意思不明
  privateRuntimeConfig: {
    axios: {
      baseURL: process.env.BASE_URL
    }
  },

  // 代理服务器
  proxyTable: {
    '/api/': {
      target: 'https://www.baidu.com/api',  // 线上地址
      pathRewrite: { '^/api/': '' }
    }
  },

  /*
  ** Build configuration
  */
  build: {
    transpile: [/^element-ui/],
    /*
    ** You can extend webpack config here
    */
    extend( config, { isDev, isClient }) {
    }
  },
  // 中间件
  serverMiddleware: [
    '~/middleware/cookie.js'
  ]
}
```

###  获取win
if (process.browser) {
 逻辑
}

### layouts文件夹
layouts文件夹：是放置内容的，公共的部分可以在这个位置去引入一个组件，公共的头，footer等
错误页也是在此文件夹内，error.vue,官网有详细写法
这里定义的东西是全局都使用的，慎用

### 自定义页面布局
在layouts中定义一个页面比如cishi.vue
```vue
<template>
    <div>
      layouts
      <nuxt/>
    </div>
</template>
<script>
  export default {
    name: 'ceshi',
  }
</script>
```
如何使用呢，在page文件内的.vue文件内加入
```vue
<template>
<div>
  asdsadas
</div>
</template>

<script>
  export default {
    layout: 'ceshi', //重要的是这个代码和上面定义的页面相关联
    name: 'ceshi'
  }
</script>
```

### 错误页面的定义
举一个个性化错误页面的例子 layouts/error.vue:
```vue
<template>
  <div class="container">
    <h1 v-if="error.statusCode === 404">页面不存在</h1>
    <h1 v-else>应用发生错误异常</h1>
    <nuxt-link to="/">首 页</nuxt-link>
  </div>
</template>

<script>
export default {
  props: ['error'],
  layout: 'blog' // 你可以为错误页面指定自定义的布局
}
</script>
```

### 关于页面的
页面组件实际上是 Vue 组件，只不过 Nuxt.js 为这些组件添加了一些特殊的配置项（对应 Nuxt.js 提供的功能特性）以便你能快速开发通用应用。
```vue
<template>
  <h1 class="red">Hello {{ name }}!</h1>
</template>

<script>
export default {
// 每次在加载组件之前调用 - 初始加载数据在这位置请求，（动作方法不在这）
  asyncData (context) {return { name: 'World' }},
  fetch () {},// fetch方法用于在呈现页面之前填充存储
  head () {},// 为此页设置元标记
  layout(){},//指定当前页面使用的布局（layouts 根目录下的布局文件）
  middleware(){},//中间件
  ...
}
</script>

<style>
.red {
  color: red;
}
</style>
```

### asyncData,fetch
这里需要一个axios请求，等会会写道如何进行网络请求,需要服务器端加载的数据支持seo优化的信息要全部渲染到这个位置
```js
//这里接收一个上下文的对象；app//可以结构出stort，和自定义的插件内导出全局的方法，route//路由，params//路由参数，query，req，res，等等。见网址：https://www.nuxtjs.cn/api/context
async asyncData ({app}) {//结构app进来
      let {$api} = app//app进行结构初api全局暴漏的一个api的方法，请求中自己封装的
      // 下面试简单的axios请求，安装axios即可使用 
      let { data } = await axios.get('https://www.baidu.com/api/v1/config-info')

      //使用 Promise.all 结果是个数组$api.getHotType(id1[0].class_id)
      let obj = await Promise.all([ $api.getHotType(id1[0].class_id) , $api.getHotType(id1[1].class_id) , $api.getHotType(id1[2].class_id) , $api.getHotType(id1[3].class_id)])
              
      //请求banner下面的数据 footer上面的 使用封装好的请求content进行接收 - 建议使用结构的方式
      //$api.getListData()后面不要.then,直接结构data即可
      let {data:content}  = await $api.getListData()
      return {//把数据return出去在template可以直接使用 - 最好在data中处理之后在使用，<div>{{info}}</div>
        info: data.data,
      }
    }
```

### fetch 方法
fetch 方法用于在渲染页面前填充应用的状态树（store）数据， 与 asyncData 方法类似，不同的是它不会设置组件的数据。
类型： Function
如果页面组件设置了 fetch 方法，它会在组件每次加载前被调用（在服务端或切换至目标路由之前）。
fetch 方法的第一个参数是页面组件的上下文对象 context，我们可以用 fetch 方法来获取数据填充应用的状态树。为了让获取过程可以异步，你需要返回一个 Promise，Nuxt.js 会等这个 promise 完成后再渲染组件。
警告: 您无法在内部使用this获取组件实例，fetch是在组件初始化之前被调用
```js
async fetch ({ store, params }){
    let { data } = await axios.get('https://www.baidu.com/stars')
    store.commit('setStars', data)//给vuex发送一个数据
  }
```

### 网络请求 - 中间件 - 用户管理，获取token - 可以不进行使用
```shell
npm install js-cookie --save
```
保存token的插件，可以使用中间件来使用，使用方法：
```js
const Cookies = require("js-cookie")  
// 创建一个在整个网站上有效的cookie
Cookies.set('name', 'value');

// 创建一个从现在起7天到期的cookie，该cookie在整个网站上均有效：
Cookies.set('name', 'value', { expires: 7 });

// 创建一个有效的cookie，该cookie对当前页面的路径有效：
Cookies.set('name', 'value', { expires: 7, path: '' });
```
#### 封装
```js
import Cookies from 'js-cookie'
const TokenKey = 'token'
// 获取
export function getToken() {
  return Cookies.get(TokenKey)
}
// 写入
export function setToken(token) {
  return Cookies.set(TokenKey, token)
}
// 删除
export function removeToken() {
  return Cookies.remove(TokenKey)
}
```
--------------------------------------------------
```shell script
npm install cookie-parser
```
首先获取中间件，就是用户登录的时候会把token保存到cookies这里面，之后要获取token活得用户的信息，登录状态等等，
安装一个cookies格式化工具方便操作 ：npm i serve cookie-parser 之后在middleware（中间件）这个文件夹内新建文件进行导入
我新建的cokie.js 内容是：
```js
module.exports = require('cookie-parser')()
```
导入npm的之后在nuxt.confige.js中进行导入中间件
```js
 // 中间件
  serverMiddleware: [
    '~/middleware/cookie.js'
  ]
```

### 中间件 - 官网说明
中间件允许您定义一个自定义函数运行在一个页面或一组页面渲染之前。

每一个中间件应放置在 middleware/ 目录。文件名的名称将成为中间件名称(middleware/auth.js将成为 auth 中间件)。

一个中间件接收 context 作为第一个参数：
```js
export default function (context) {
  context.userAgent = process.server ? context.req.headers['user-agent'] : navigator.userAgent
}
```

中间件执行流程顺序：

nuxt.config.js
匹配布局
匹配页面
中间件可以异步执行,只需要返回一个 Promise 或使用第2个 callback 作为第一个参数：

middleware/stats.js

import axios from 'axios'
```js
export default function ({ route }) {
  return axios.post('http://my-stats-api.com', {
    url: route.fullPath
  })
}
```
然后在你的 nuxt.config.js 、 layouts 或者 pages 中使用中间件:

nuxt.config.js
```js
module.exports = {
  router: {
    middleware: 'stats'
  }
}
```

现在，stats 中间件将在每个路由改变时被调用。

您也可以将 middleware 添加到指定的布局或者页面:

pages/index.vue 或者 layouts/default.vue
```js
export default {
  middleware: 'stats'
}

```

### axios网络请求 - https://zh.nuxtjs.org/guide/async-data
安装axios - npm install @nuxtjs/auth @nuxtjs/axios
在plugibns中新建文件api.js之后写入axios的网络请求配置，为什么在plugins中，因为这个位置可以获取到上下文对象，可以结构初store和app，以及获取win和dom
别的位置暂时没有找到可以结构出的，所以放在这个插件文件夹下了 - 默认导出的配置
 - 全局都可以使用直接在asyncData中$api.getInfo()，在vue其他方法中this.$api.getInfo(); 
inject全局暴漏导出
```js
import apiAll from '../api/apiAll.js'//导入请求的n个地址 地址自定义
export default function ({ app }, inject) {
  let { store, $axios } = app//结构出store, $axios - 自带的axios
  $axios.defaults.baseUrl = '/api'//设置api，nuxt.confige.js中以解释/api的来历
  /** 拦截请求设置 可以做一些请求之前的操作 token **/
  $axios.interceptors.request.use(
    function (config) {
    //添加请求的头部，store.state.access_token这个是vuex中保存的token下面会解释到

    //这个位置可以获取浏览器的win所以可以读取token，在请求拦截之前把token添加进去
    //浏览器没有的话，说明不是登录状态，判断用户是否登录
// if (process.browser){}这个是获取浏览器的win和dom
      let Token = ''
      if (process.browser) {
        Token = window.localStorage.getItem('token')
      }

      config.headers['Authorization'] = 'Bearer ' + Token
      // 在发送请求之前做些什么
      return config
    },
    function (error) {
      // 对请求错误做些什么
      return Promise.reject(error)
    }
  )
// 2.2.响应拦截
   $axios.interceptors.response.use(res => {
//获取的数据返回一个data
      return res.data
    }, err => {
      return err.data
    })

  inject('api', apiAll($axios))//inject全局暴漏，名字为api 导出的是这个axios
}
```
apiAll.js  请求集合，自定义的地址，和上面进行对应
```js
import homev2 from './homev2' //可以多个文件导入
import institutionalUserv2 from './institutionalUserv2'

export default function ($axios) {
  return {
    ...homev2($axios),//遍历导入
    ...institutionalUserv2($axios)
  }
}
```
homev2的文件:   请求分开的文件
```js
/*url是个字符串可拼接
axios.request(config)
axios.get(url[, config])
axios.delete(url[, config])
axios.head(url[, config])
axios.options(url[, config])
axios.post(url[, data[, config]])
axios.put(url[, data[, config]])
axios.patch(url[, data[, config]])
*/
export default function ($axios) {
  return {
    /**
     * 获取数据
     * */
    getListData () {
      return $axios.get('/v1/index')
    },
    /**
     *  热门分类 /v1/index/hot-type
     * */
    getHotType(id,data) {
      return  $axios.post("/v1/index/hot-type?id=" + id,{
        name:data.name
        })
    },
    /**
     * 传输图片，fil格式，前提提前转froomData格式
     * */
    uImage(data) {
          return $axios.post('/v1/file/images',data)
        }
  }
}

```

### vuex获取用户信息
如何存储token
if(process.browser){//获取浏览器对象cookie
  document.cookie = 'token = ' + access_token;
}
如何清除token
var keys = document.cookie.match(/[^ =;]+(?=\=)/g)
if (keys) {
  for (var i = keys.length; i--;)
    document.cookie = keys[i] + '=0;expires=' + new Date(0).toUTCString()
}

window.localStorage.setItem('token',token)//存
window.localStorage.getItem('token',token)//取
window.localStorage.setItem('user', JSON.stringify(content.data))//json存储
JSON.parse(json);　　//解析为JSON对象
```js
export const state = () => ({
  access_token: '',

})

export const mutations = {
  //保存access_token
  ACCESS_TOKEN(state, playLoad) {
    state.access_token = playLoad
  },
}
export const actions = {
  //每次页面加载的时候就会执行这个函数，之后可以获取到cookies保存的值，之后可以保存到vuex中，
  //通过plugins文件新建api文件进行网络请求以及axios的拦截等
  async nuxtServerInit({ commit }, { req,app }) {
    try {
      let { token } = req.cookies //用户登录保存token
      if (token) {//如果用户登录，这里就是true

        //将token报错到stat中，使用mutations内的commit方法进行保存
        commit('ACCESS_TOKEN', token)

        // 网络请求 通过token获取用户信息保存到vuex中去 - 页面刷新会有问题
        let { data }  = await app.$api.getUserInfo(token)
        commit('USER', data)
      }else {
      }
    }catch (e) {
      console.log(e)
    }
  },
}
```

### 路由
是根据文件的目录生成的路由_id.vue可以传输路由的参数，如果想有链接的标识，鼠标经过的时候下面显示连接使用标签：<nuxt-link to="/about">关于</nuxt-link>
####  路由问题
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
你会发现名称为users-id的路由路径带有:id？参数，表示该路由是可选的。原理跟vue动态路由是一致的。
   使用跳转的时候都是用nuxt-link或者this.$router.push({path:''})，这样的话可以使用vuex
```html
<nuxt-link :to="'/users/' + id?name='四叶草'">首页</nuxt-link>
```

#### 动态路由
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
使用跳转的时候都是用nuxt-link或者this.$router.push({path:''})，这样的话可以使用vuex
```html
<nuxt-link :to="'/users/' + id?name='四叶草'">首页</nuxt-link>
```

### 插件使用
需要详细阅读官方文章： - https://zh.nuxtjs.org/guide/plugins
之前默认安装element 安装富文本，图片裁切 地图，boot栅格，可以使用vue add方法进行安装，也可以npm i -d ...安装
安装方法安装富文本图片裁切：npm install -d vue-quill-editor quill vue-cropper 
官网：
vue-quill-editor：https://quilljs.com/ 
git：https://github.com/surmon-china/vue-quill-editor
vue-cropper：https://github.com/xyxiao001/vue-cropper
需要在plugins文件夹内注册插件
注册插件：plugins文件夹内 新建js文件 - 例子：如果有导入的css的话，建议放到os上使用cdn进行引入
```js
import Vue from 'vue'
import VueQuillEditor from 'vue-quill-editor'
Vue.use(VueQuillEditor)
``` 
需要nuxt.config.js内进行设置
```shell
// nuxt.config.js
  plugins: [
    '@/plugins/element-ui',//element
    '@/plugins/api',//封装的axios
    '@/plugins/bootstrap-vue',//栅格
    '@/plugins/vue-moment',//格式化时间
    { src: '@/plugins/map', ssr: false },//地图
    { src: '@/plugins/vue-quill-editor', ssr: false },//富文本
    { src: '@/plugins/vue-cropper', ssr: false },//裁切
    { src: '@/plugins/md5', ssr: false }//md5加密
  ],
```


### 新建一个isMobile.js文件，路径自选，我的路径是/static/js/isMobile.js
```js
(function() {
    console.log("判断移动端");
    var sUserAgent = navigator.userAgent.toLowerCase();
    if (/ipad|iphone|midp|rv:1.2.3.4|ucweb|android|windows ce|windows mobile/.test(sUserAgent)) {
        console.log("移动端");
        //跳转移动端页面
        window.location.replace("xxxx");//跳转后没有后退功能 
    } else {
        console.log("PC端");
    }
})();
```

### 在nuxt.config.js加上对应配置
```json
head: {
    script:[  
      {src:'js/isMobile.js'},
    ]
  },
```
重新启动项目可以看到效果

### 打包整理服务器端上线
首先npm run dev 会生成一个dist和.nuxt文件，用到的是nuxt，nuxt.config.js，static，package.json四个文件
之后本地服务器端运行 npm start



### 富文本安装和图片裁切 - 最好安装element 
安装富文本quill 裁切图片：vue-cropper  
npm install -d vue-quill-editor quill vue-cropper
在插件目录下全局引入 - 新建js文件，可以分开，可以合在一起 
例子：
```js
import Vue from 'vue'
import VueQuillEditor from 'vue-quill-editor'
Vue.use(VueQuillEditor)
```
全局引用：nuxt.config.js配置
```js
  plugins: [
    '@/plugins/element-ui',
    '@/plugins/bootstrap-vue',
    { src: '@/plugins/map', ssr: false },
    { src: '@/plugins/vue-quill-editor', ssr: false },
    { src: '@/plugins/vue-cropper', ssr: false }
  ],
```
具体使用富文本 - nuxt使用富文本图片裁切以及element
图片裁切 - nuxt单个裁切图片和多个裁切

###  百度地图的使用 
安装vue-baidu-map
```
npm i vue-baidu-map -D
```
在plugins新建map.js:
```
import BaiduMap from 'vue-baidu-map'
import Vue from 'vue'
Vue.use(BaiduMap, {
  ak: '申请的百度地图密匙'
})
```
直接去 - Nuxt.js使用百度地图vue-baidu-map
