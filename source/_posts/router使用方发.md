---
uuid: 77af8edb-0772-de4b-9c28-5915c4fae616
title: $router使用方发
date: 2020-03-28 11:38:10
tags: vue
category: vue
---
当vue打包的时候打包生成的路由路径打不开，调整router中的//mode: "history",，将他注释，是不让url指向发生改变
##  常识：
返回上一层：
this.$router.go(-1)

tag=“渲染的标签名”
to=“模板名字”
linkActiveClasss：“active”    //修改点击切换的时候的class
active-class=“active”   //默认选中的class修改
```
<router-link to="/home">首页</router-link>//防止模板的标签，to=“模板名字”
<router-link to="/About">关于</router-link>//防止模板的标签，to=“模板名字”
<router-view></router-view>//模板里面内容显示的区域
```

##  拿到路由对象
$router拿到最大的路由
this.$router.方法如：replaceStateurl监听
$route拿到当前路由的数据

##  导航守卫：
前置钩子（守卫guard）：beforeEach
	函数类型，函数内包含三个参数to,from,next,next()是必须有的router.beforeEach((to,from,next) =>{})
后置的钩子（钩子hook）：afterEach
	函数类型，函数内有两个参数，to,from；router.beforeEach((to,from) =>{})

##  缓存数据
keep-ailve--清除缓存每次点击重新创建
```
<keep-alive exclude="创建模板的name,name2,name3">//那些不缓存
```

### router：

```
//创建导入，总结整合目录这个是起一个项目的基本
/*
*找到对应的vue文件命名一下，之后使用
*/
const Home = () => import('views/home/Home');

//创建与个router
const routes = [

//默认的路径，打开直接是/home
  {
    path:'/',
    redirect:'/home'
  },

  //导入的目录
  {
    path: '/home',
    name: 'Home',
    component: Home
  }

//详情页传递，因为:iid可以传来参数更改url的请求，可以通过vue axios来更改这个值
  {
    path: '/detail/:iid',
    name: 'Detail',
    component: Detail
  },
]

//实例化
const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

//导出
export default router

```

* vue axios接收参数

```
import { request } from "./request";

export function getDetailActive(iid) {
  return request({
    url: '/detail',

    //拼接的参数
    params: {
      iid
    }
  })
}

```

* 如何传入axios接收参数：

```
//生命周期 - 创建完成（可以访问当前this实例）
created() {
  //
  const iid = this.$route.params.iid

  //axios发来的getDetailActive需要传参
    getDetailActive(iid).then(res => {
      console.log(res)
    })
},
```

* 如何改变url路径

```
//监听事件····detail在router中定义过后缀名一定同一（iid）
 this.$router.push('/detail/' + '找到iid，或者唯一的id')
```