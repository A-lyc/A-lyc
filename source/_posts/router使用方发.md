---
uuid: 77af8edb-0772-de4b-9c28-5915c4fae616
title: $router使用方法
date: 2020-03-28 11:38:10
tags: vue
category: vue
---
我们可以在任何组件内通过 this.$router 访问路由器，也可以通过 this.$route 访问当前路由：
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


### 官方文档说明
我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。例如，我们有一个 User 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。那么，我们可以在 vue-router 的路由路径中使用“动态路径参数”(dynamic segment) 来达到这个效果：

```
const User = {
  template: '<div>User</div>'
}

const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
```
现在呢，像 /user/foo 和 /user/bar 都将映射到相同的路由。

一个“路径参数”使用冒号 : 标记。当匹配到一个路由时，参数值会被设置到 this.$route.params，可以在每个组件内使用。于是，我们可以更新 User 的模板，输出当前用户的 ID：
```
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
```
你可以看看这个在线例子。

你可以在一个路由中设置多段“路径参数”，对应的值都会设置到 $route.params 中。例如：

模式	匹配路径	$route.params
```
/user/:username	/user/evan	{ username: 'evan' }
/user/:username/post/:post_id	/user/evan/post/123	{ username: 'evan', post_id: '123' }
```
除了 $route.params 外，$route 对象还提供了其它有用的信息，例如，$route.query (如果 URL 中有查询参数)、$route.hash 等等。你可以查看 API 文档 的详细说明。


### 使用编程式的添加路由
如果提供了 path，params 会被忽略，上述例子中的 query 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 name 或手写完整的带有参数的 path：
```
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

router.replace(location, onComplete?, onAbort?)
跟 router.push 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录。

声明式	编程式
<router-link :to="..." replace>	router.replace(...)
#router.go(n)

### 命名路由
有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。你可以在创建 Router 实例的时候，在 routes 配置中给某个路由设置名称。
```
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
要链接到一个命名路由，可以给 router-link 的 to 属性传一个对象：

<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
这跟代码调用 router.push() 是一回事：
```
router.push({ name: 'user', params: { userId: 123 }})
这两种方式都会把路由导航到 /user/123 路径

### 传参
你可以创建一个函数返回 props。这样你便可以将参数转换成另一种类型，将静态值与基于路由的值结合等等。
```
const router = new VueRouter({
  routes: [
    { path: '/search', component: SearchUser, props: (route) => ({ query: route.query.q }) }
  ]
})
```
URL /search?q=vue 会将 {query: 'vue'} 作为属性传递给 SearchUser 组件。

请尽可能保持 props 函数为无状态的，因为它只会在路由发生变化时起作用。如果你需要状态来定义 props，请使用包装组件，这样 Vue 才可以对状态变化做出反应。
