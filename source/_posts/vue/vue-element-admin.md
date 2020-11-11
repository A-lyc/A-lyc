---
uuid: bea548e6-eb93-0eb2-d70a-a0b89753cfdd
title: vue-element-admin
date: 2020-03-28 21:00:25
tags: [vue-element-admin,]
category: vue
---
这是一个极简的 vue admin 管理后台。它只包含了 Element UI & axios & iconfont & permission control & lint，这些搭建后台必要的东西
## 安装方法
这是一个极简的 vue admin 管理后台。它只包含了 Element UI & axios & iconfont & permission control & lint，这些搭建后台必要的东西。
下载安装vue-elenemt-admin 下载admin和template，需要使用npm进行安装使用,访问地址：https://panjiachen.gitee.io/vue-admin-template/#/login?redirect=%2Fdashboard
```shell
// vue-elenemt-admin
# 克隆项目
git clone https://github.com/PanJiaChen/vue-element-admin.git

# 进入项目目录
cd vue-element-admin

# 安装依赖
npm install

# 建议不要用 cnpm 安装 会有各种诡异的bug 可以通过如下操作解决 npm 下载速度慢的问题
npm install --registry=https://registry.npm.taobao.org

# 本地开发 启动项目
npm run dev
```
```shell
// vue-admin-template
# 克隆项目
git clone https://github.com/PanJiaChen/vue-admin-template.git

# 进入项目目录
cd vue-admin-template

# 安装依赖
npm install

# 建议不要直接使用 cnpm 安装以来，会有各种诡异的 bug。可以通过如下操作解决 npm 下载速度慢的问题
npm install --registry=https://registry.npm.taobao.org

# 启动服务
npm run dev
```
### 发布
```shell
# 构建测试环境
npm run build:stage

# 构建生产环境
npm run build:prod
```
### 其他
```shell
# 预览发布环境效果
npm run preview

# 预览发布环境效果 + 静态资源分析
npm run preview -- --report

# 代码格式检查
npm run lint

# 代码格式检查并自动修复
npm run lint -- --fix
```

## 登录注册
删除一些请求，在vue.config.js中删除，删除之后会导致登录不上去的，
```shell
before: require('./mock/mock-server.js')
```
### 配置跨域
```shell
  devServer: {
    proxy: {
      '/dev-api': { // 重点
        target: 'http://localhost:3000',// 重点
        ws: true,
        changeOrigin: true,
        pathRewrite: {
          '^/dev-api': ''
        }
      }
    },
    port: port,
    open: true,
    overlay: {
      warnings: false,
      errors: true
    }
  },
```
### 请求
请求是在vuex中发出的之后保存token到浏览器，由于在router的导航首位中重复获取所以保证了一直处于登录状态
请求之前保证数据库内有这个用户
```shell
// login - index
this.$store.dispatch('user/login', this.loginForm).then(() => {
        this.$router.push({ path: this.redirect || '/' })
        this.loading = false
      }).catch(() => {
        this.loading = false
      })

//  store - modules - user
// 导入了请求方式
import { login, logout, getInfo } from '@/api/user'
import { getToken, setToken, removeToken } from '@/utils/auth'
  login({ commit }, userInfo) {
    const { username, password } = userInfo
    return new Promise((resolve, reject) => {
      // 网络请求 .trim()去除空格
      login({ username: username.trim(), password: password }).then(response => {
        const { data } = response
        console.log('---- 登录成功 ----')
        commit('SET_TOKEN', data.token)
         // 保存token 方便使用token来获取用户的信息
        setToken(data.token)
        resolve()
      }).catch(error => {
        reject(error)
      })
    })
  },
// 获取用户信息
 // get user info
  getInfo({ commit, state }) {
    return new Promise((resolve, reject) => {
      // 网络请求获取数据
      getInfo(state.token).then(response => {
        console.log('---- 获取用户信息 ----')
        const { data } = response
        if (!data) {
          return reject('验证失败，请重新登录。')
        }
        const { name, avatar } = data
        // 更改数据
        commit('SET_NAME', name)
        commit('SET_AVATAR', avatar)
        resolve(data)
      }).catch(error => {
        reject(error)
      })
    })
  },
```
### axios请求
在api文件夹中创建user.js之后关于用户的请求在这
```shell
import request from '@/utils/request'

// 传输账号密码，获取token
export function login(data) {
  return request({
    url: '/users/login',
    method: 'post',
    data
  })
}
```

## layout自己理解的机制
在layout文件夹：是一个公共文件这个文件属于公共信息，之后可以直接引入
类似于nuxt.js
这个layout作为父级其他的作为子集
```shell
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router)
/* Layout */
import Layout from '@/layout'
  {
    path: '/example', // 名称
    component: Layout, // 父组件 组件
    redirect: '/example/table', // 重定向
    name: 'example',
    meta: { title: '展示列表', icon: 'el-icon-s-help' },
    children: [ // 组件的孩子
      {
        path: 'edit',
        name: 'edit',
        component: () => import('@/views/table/edit'),
        meta: { title: '新闻添加', icon: 'el-icon-s-help' }
      },
      {
        path: 'table',
        name: 'table',
        component: () => import('@/views/table/index'),
        meta: { title: '新闻列表', icon: 'table' }
      }
    ]
  },
  {
    path: '/nested',
    component: Layout,
    redirect: '/nested/menu1',
    name: 'nested',
    meta: {
      title: '内容',
      icon: 'nested'
    },
    children: [
      {
        path: 'menu1',
        component: () => import('@/views/nested/menu1/index'), // Parent router-view
        name: 'menu1',
        meta: { title: 'Menu1' },
        children: [
          {
            path: 'menu1-2',
            component: () => import('@/views/nested/menu1/menu1-2'),
            name: 'menu1-2',
            meta: { title: '菜单-2' },
            children: [
              {
                path: 'menu1-2-1',
                component: () => import('@/views/nested/menu1/menu1-2/menu1-2-1'),
                name: 'menu1-2-1',
                meta: { title: '菜单-2-1' }
              },
              {
                path: 'menu1-2-2',
                component: () => import('@/views/nested/menu1/menu1-2/menu1-2-2'),
                name: 'menu1-2-2',
                meta: { title: '菜单-2-2' }
              }
            ]
          }
        ]
      },
    ]
  },

```

<a href="#"></a>

## h2全局导航首位 router
在这个文件permission.js
组件生命周期操作路由
```shell
  // 路由进入的时候
  beforeRouteEnter(to, from, next) {
    console.log('---- 4 ----')
    console.log(to)
    console.log(from)
    next()
  },
  // 路由离开之前
  beforeRouteLeave(to, from, next) {
    console.log('---- 5 ----')
    console.log(to)
    console.log(from)
    next()
  },
```

## 例子template
template自己定义模板这个需要有后端的支持
<a href="./newAdmin.zip" target="_blank"> template自己定义模板 </a>
选要后端支持 npm i
<a href="./demo01.zip" target="_blank"> node+express+mongoose后端模板 </a>













