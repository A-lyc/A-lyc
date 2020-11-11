---
title: vue3.0X更新文档
date: 2020-10-24 11:04:50
tags: [vue3.0,vue]
category: vue
---
## Vue 3.0 项目初始化
## 第一步，安装 vue-cli：
```shell
npm install -g @vue/cli
```
注意以下命令是错误的！
npm install -g vue
npm install -g vue-cli

## 安装成功后，我们即可使用 vue 命令，测试方法
```shell
 $ vue -V
 @vue/cli 4.3.1
```

## 第二步，初始化 vue 项目：
```shell
vue create vue-next-test
```

## 输入命令后，会出现命令行交互窗口，这里我们选择 Manually select features：
```shell
 Vue CLI v4.3.1
 ? Please pick a preset: 
   default (babel, eslint) 
 ❯ Manually select features 
```
随后我们勾选：Router、Vuex、CSS Pre-processors 和 Linter / Formatter，
这些都是开发商业级项目必须的：
```shell
Vue CLI v4.3.1
? Please pick a preset: Manually select features
? Check the features needed for your project: 
 ◉ Babel
 ◯ TypeScript
 ◯ Progressive Web App (PWA) Support
 ◉ Router
 ◉ Vuex
 ◉ CSS Pre-processors
❯◉ Linter / Formatter
 ◯ Unit Testing
 ◯ E2E Testing
```
```shell
注意：Vue 3.0 项目目前需要从 Vue 2.0 项目升级而来，所以为了直接升级到 Vue 3.0 全家桶，
我们需要在 Vue 项目创建过程中勾选 Router 和 Vuex，所以避免手动写初始化代码
```

## 升级 Vue 3.0 项目
目前创建 Vue 3.0 项目需要通过插件升级的方式来实现，

vue-cli 还没有直接支持，我们进入项目目录，并输入以下指令：

```shell
cd vue-next-test
vue add vue-next
```
执行上述指令后，会自动安装 vue-cli-plugin-vue-next 插件（查看项目代码），该插件会完成以下操作：

安装 Vue 3.0 依赖
更新 Vue 3.0 webpack loader 配置，使其能够支持 .vue 文件构建（这点非常重要）
创建 Vue 3.0 的模板代码
自动将代码中的 Vue Router 和 Vuex 升级到 4.0 版本，如果未安装则不会升级
自动生成 Vue Router 和 Vuex 模板代码
完成上述操作后，项目正式升级到 Vue 3.0，

注意该插件还不能支持 typescript，用 typescript 的同学还得再等等。（就是目前还不太支持TS）


## 创建路由 router/index
```shell
import { createRouter, createWebHashHistory } from 'vue-router'
import Home from '../views/Home.vue'

const routes = [
  {
    path: '/home',
    name: 'Home',
    component: Home,// 父组件模板
    redirect: '/dashboard', // 重定向
    children:[{
      path: 'dashboard',// 连接是/home/dashboard
      name: 'Dashboard', // 和模板文件最好对应
      component: () => import('@/views/dashboard/index'), // 模板文件路径
      meta: { title: '标题', icon: 'dashboard' } // mate标签， 可做权限设置
    }]
  },
  {
    path: '/about',
    name: 'About',
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router
```
初始化 Vue Router 的过程与 3.0 版本变化不大，只是之前采用构造函数的方式，
这里改为使用 createRouter 来创建 Vue Router 实例，
建一个公共的layout文件是一个公共的文件header和footer公用 都是home的子文件所以改变的是router-view这个状态
```shell
<template>
    <div>
        <h1>header</h1>
        <div class="nav">
            <router-link to="/home/dashboard">首页</router-link> |
            <router-link to="/home/index">关于我们</router-link> |
            <router-link to="/home/Content">内容 - Content</router-link>
        </div>
        ==========================>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
        <router-view :key="key"/>
        ==========================>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
        <h1>footer</h1>
        <div class="nav">
            <router-link to="/">首页</router-link> |
            <router-link to="/about/index">关于我们</router-link> |
            <router-link to="/about/Content">内容 - Content</router-link>
        </div>
    </div>
</template>
```
app.vue
```shell
<template>
  <div id="app">
    <router-view />
  </div>
</template>
```

## 启动项目：
```shell script
npm run serve
```

## 状态和事件绑定
Vue 3.0 中定义状态的方法改为类似 React Hooks 的方法，下面我们在 Test.vue 中定义一个状态 count：

```shell
<template>
  <div class="test">
    <h1>test count: {{count}}</h1>
  </div>
</template>

<script>
  import { ref } from 'vue'

  export default {
    setup () {
      const count = ref(0)
      return {
        count
      }
    }
  }
</script>
```

## Vue 3.0 中初始化状态通过 setup 方法，
定义状态需要调用 ref 方法。接下来我们定义一个事件，用来更新 count 状态：
```shell
<template>
  <div class="about">
<input type="text" v-model="text">
        <h3>{{text}}</h3>
    <h1>{{count02}}</h1>
    <h1> 关于我们 {{count}}</h1>
    <div>count,
      test： {{test}},
      doubleCount：{{doubleCount}},
      a：{{a}}
    </div>
    <h1 @click="add">add</h1>
    <h1 @click="update">update</h1>
    <h1 @click="getgreet">methods ====>>>> getgreet</h1>
  </div>
</template>
<script>

  import { ref, computed, watch, getCurrentInstance,
    onMounted,onRenderTracked,onRenderTriggered,
    onBeforeMount,onBeforeUpdate,onUpdated,onBeforeUnmount,onUnmounted,
    onErrorCaptured
  } from "vue";

  export default {
    name:'about',
    // 初始化数据使用，生命周期使用 需要在里面定义请求函数，来请求初始化数据
    // 监听不到data和methods内的数据
    setup() {
      // 方法获取当前组件的实例
      console.log(getCurrentInstance());
      const { ctx } = getCurrentInstance(); // 获取当前实例
      onBeforeMount(()=>{
        console.log("在挂载开始之前被调用")
      })
      onRenderTracked(() => {
        console.log('渲染跟踪');
      });
      onBeforeUnmount(()=>{
        console.log("实例销毁之前调用")
      })
      onBeforeUpdate(()=>{
        console.log("数据更新时调用")
      })
      onUnmounted(()=>{
        console.log("组件已完成了销毁")
      })
      onErrorCaptured(()=>{
        console.log("在错误捕获")
      })
      onUpdated(()=>{
        console.log("页面也完成了更新")
      })
      onMounted(() => {
        console.log("挂载后 >>>>>>01");
      });
      onRenderTriggered(() => {
        console.log('渲染 - 触发')
      });

      // 页面加载的时候触发
      const count = ref(0);
      const count02 = ref('文字');
      const text = ref('文字');
      let test = ref("我们都是好孩子"); // 定义test默认显示内容
      // 获取当前路由
      console.log(ctx.$router.currentRoute.value);
      // 页面加载前计算属性获取vuex上的属性
      const a = computed(() => ctx.$store.state.test.a);
      const update = () => {
        // 修改vuex的信息
        ctx.$store.commit("setTestA", count.value * 10);
        count.value = count.value*10
        console.log(ctx.$store.state.test.a);
      };
      const add = () => {
        // 点击动作
        test.value = "我是好人"; // 修改值
        count.value++; // count加一
      };
      watch(() => {
                // 页面加载就读取这个信息 监听属性的变化
                console.log("---- 页面加载就读取这个信息 监听属性的变化 ----");
                console.log(count);
                count.value;
              },(val) => {
                console.log("---- 页面加载就读取这个信息 ----");
                console.log(`count is ${val}`);
              }
      );
      // 计算属性获取和写入
      const doubleCount = computed(()=>{
        // 计算属性获取 count.value * 2
        return count.value * 2;
      });

      return {
        // 和vue2的data 默认在上面ref('')定义 返回定义的对象
        count,
        count02,
        test,
        doubleCount,
        add,
        a,
        update,
        text
      };
    },
    // 数据改变的时候使用，事件数据，表单数据 - 和原先的data一样
    data(){
      return {

      }
    },
    mounted() {
      // 这个比上面的on要晚
      console.log('挂载后 >>>>>>02')
    },
    methods: {
      getgreet() {
        console.log("---- methods的点击动作 ----");
        this.count02 = "四叶草02"
        console.log(this.doubleCount)
        this.count = 10
        console.log("---- end methods的点击动作 ----");
      },
    },
    watch:{
      count(old,newVal){
        console.log(old,newVal)
      }
    }
  };
</script>

```

这里的 add 方法不再需要定义在 methods 中，
但注意更新 count 值的时候不能直接使用 count++，而应使用 count.value++，
更新代码后，点击按钮，count 的值就会更新了：

## 计算属性和监听器
Vue 3.0 中计算属性和监听器的实现依赖 computed 和 watch 方法：

```shell
<template>
  <div class="test">
    <h1>test count: {{count}}</h1>
    <div>count * 2 = {{doubleCount}}</div>
    <button @click="add">add</button>
  </div>
</template>

<script>
  import { ref, computed, watch } from 'vue'

  export default {
    setup () {
      const count = ref(0)
      const add = () => {
        count.value++
      }
      watch(() => count.value, val => {
        console.log(`count is ${val}`)
      })
      const doubleCount = computed(() => count.value * 2)
      return {
        count,
        doubleCount,
        add
      }
    }
  }
</script>

```

计算属性 computed 是一个方法，里面需要包含一个回调函数，当我们访问计算属性返回结果时，会自动获取回调函数的值：
```shell
const doubleCount = computed(() => count.value * 2)
```
监听器 watch 同样是一个方法，它包含 2 个参数，2 个参数都是 function：

```shell script
watch(() => count.value, 
  val => {
    console.log(`count is ${val}`)
  })
  ```
  
第一个参数是监听的值，count.value 表示当 count.value 发生变化就会触发监听器的回调函数，即第二个参数，第二个参数可以执行监听时候的回调

如果是2 个以上的监听属性 就这样
```
watch(
  [refA, () => refB.value],
  ([a, b], [prevA, prevB]) => {
    console.log(`a is: ${a}`)
    console.log(`b is: ${b}`)
  }
)
 ```

## 获取路由
Vue 3.0 中通过 getCurrentInstance 方法获取当前组件的实例，然后通过 ctx 属性获得当前上下文，

ctx.$router 是 Vue Router 实例，里面包含了 currentRoute 可以获取到当前的路由信息

```
<script>
  import { getCurrentInstance } from 'vue'

  export default {
    setup () {
      const { ctx } = getCurrentInstance()
      console.log(ctx.$router.currentRoute.value)
    }
  }
</script>
```

## axios
配置axios
```js
const axios = require('axios')

//git请求
export function request(config) {
    // 1.创建axios的实例
    const instance = axios.create({
        baseURL: '/api',
    })

    // 2.axios的拦截器
    // 2.1.请求拦截的作用
    instance.interceptors.request.use(config => {
        console.log("请求拦截器")
        return config
    },err => {
        return err.data
    })

    // 2.2.响应拦截
    instance.interceptors.response.use(res => {
        console.log("响应拦截")
        return res.data
    },err => {
        return err.data
    })

    // 3.发送真正的网络请求
    return instance(config)
}
```
vue.confing.js - 跨域配置
```js

module.exports = {
    devServer: {
        proxy: {
            '/api': {
                target: 'http://localhost:3000/', //接口域名
                changeOrigin: true,             //是否跨域
                ws: true,                       //是否代理 websockets
                secure: true,                   //是否https接口
                pathRewrite: {                  //路径重置
                    '^/api': ''
                }
            }
        }
    }
};
```
配置请求方式
```js
import {request} from "./index";

export function query(){
    return request({
        url: '/goods',
        method:'get'
    })
}
export function add(){
    return request({
        url: '/goods/add',
        method:'get'
    })
}
export function del(id){
    return request({
        url: `/goods/del?id=${id}`,
        method:'get'
    })
}
export function amend(data){
    return request({
        url: `/goods/amend`,
        data:data,
        method:'post'
    })
}

export function upImage(data){
    return request({
        url: `/goods/upImage`,
        data:data,
        method:'post'
    })
}

```
组件中使用
```js
 const $api = require('@/api/ceshi.js')
 $api.getHomeMultidata().then(res=>{
   message.value = res.data
 })
```

## Vuex 的集成方法如下：

定义 Vuex 状态
第一步，修改 src/store/index.js 文件：

```
import Vuex from 'vuex'

export default Vuex.createStore({
  state: {
    test: {
      a: 1
    }
  },
  mutations: {
    setTestA(state, value) {
      state.test.a = value
    }
  },
  actions: {
  },
  modules: {
  }
})
```
Vuex 的语法和 API 基本没有改变,我们在 state 中创建了一个 test.a 状态，在 mutations 中添加了修改 state.test.a 状态的方法： setTestA

## 引用 Vuex 状态
第二步，在 Test.vue 中，通过计算属性使用 Vuex 状态：

```
<template>
  <div class="test">
    <h1>test count: {{count}}</h1>
    <div>count * 2 = {{doubleCount}}</div>
    <div>state from vuex {{a}}</div>
    <button @click="add">add</button>
  </div>
</template>

<script>
  import { ref, computed, watch, getCurrentInstance } from 'vue'

  export default {
    setup () {
      const count = ref(0)
      const add = () => {
        count.value++
      }
      watch(() => count.value, val => {
        console.log(`count is ${val}`)
      })
      const doubleCount = computed(() => count.value * 2)
      const { ctx } = getCurrentInstance()
      console.log(ctx.$router.currentRoute.value)
      const a = computed(() => ctx.$store.state.test.a)
      return {
        count,
        doubleCount,
        add,
        a
      }
    }
  }
</script>
```
这里我们通过计算属性来引用 Vuex 中的状态：

const a = computed(() => ctx.$store.state.test.a)
ctx 是上节中我们提到的当前组件实例

## 更新 Vuex 状态
更新 Vuex 状态仍然使用 commit 方法，这点和 Vuex 3.0 版本一致：

```
<template>
  <div class="test">
    <h1>test count: {{count}}</h1>
    <div>count * 2 = {{doubleCount}}</div>
    <div>state from vuex {{a}}</div>
    <button @click="add">add</button>
    <button @click="update">update a</button>
  </div>
</template>

<script>
  import { ref, computed, watch, getCurrentInstance } from 'vue'

  export default {
    setup () {
      const count = ref(0)
      const add = () => {
        count.value++
      }
      watch(() => count.value, val => {
        console.log(`count is ${val}`)
      })
      const doubleCount = computed(() => count.value * 2)
      const { ctx } = getCurrentInstance()
      console.log(ctx.$router.currentRoute.value)
      const a = computed(() => ctx.$store.state.test.a)
      const update = () => {
        ctx.$store.commit('setTestA', count)
      }
      return {
        count,
        doubleCount,
        add,
        a,
        update
      }
    }
  }
</script>
```
这里我们点击 update a 按钮后，会触发 update 方法，此时会通过 ctx.$store.commit 调用 setTestA 方法，将 count 的值覆盖 state.test.a 的值

总的效果呢是这样的
文章来源于：https://www.cnblogs.com/yf-html/p/12753540.html
