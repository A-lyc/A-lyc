---
title: vue3.0X更新文档
date: 2099-10-24 11:04:50
tags: [vue3.0,vue]
category: vue
---
## Vue 3.0 项目初始化
## 第一步，安装 vue-cli：
```shell
npm install -g @vue/cli
```
 - 安装成功后，我们即可使用 vue 命令，测试方法
```shell
 $ vue -V
 @vue/cli 4.3.1
```

 - 第二步，初始化 vue 项目：
```shell
vue create vue-next-test
```

 - 输入命令后，会出现命令行交互窗口，这里我们选择 Manually select features：
```shell
 Vue CLI v4.3.1
 ? Please pick a preset: 
   default (babel, eslint) 
 ❯ Manually select features 
```
 - 随后我们勾选：Router、Vuex、CSS Pre-processors 和 Linter / Formatter，这些都是开发商业级项目必须的：
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
 - 注意：Vue 3.0 项目目前需要从 Vue 2.0 项目升级而来，所以为了直接升级到 Vue 3.0 全家桶，
我们需要在 Vue 项目创建过程中勾选 Router 和 Vuex，所以避免手动写初始化代码

## Vue 3.0 基本特性体验

 - 创建路由 router/index
```shell
import { createRouter, createWebHashHistory } from 'vue-router'
import Home from '../views/Home.vue'

const routes = [
  {
    path: '/home',
    name: 'Home',
    component: Home,// 父组件模板 Layout 机制
    redirect: '/dashboard', // 重定向
    meta: { title: '标题', icon: 'dashboard' } // mate标签， 可做权限设置
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
可以使用循环来便利路由内的标题
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
        <router-view :key="key"/> // key() {return this.$route.path},
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
app.vue   放置内容的一个
```shell
<template>
  <div id="app">
    <router-view />
  </div>
</template>
```

## watch的应用
· watch监听器可以监听一个getter函数
 - 这个getter要返回一个响应式对象
 - 当该对象更新后，会执行对应的回调函
 
```shell
import { reactive, watch } from 'vue'
const state = reactive({ count: 0 })
watch(() => state.count, (newValue, oldValue) => {
    // 因为watch被观察的对象只能是getter/effect函数、ref、热active对象或者这些类型是数组
    // 所以需要将state.count变成getter函数
})
```
· watch可以监听响应式对象
```shell
import { ref, watch } from 'vue' 
const count = ref(0) 
watch(count, (newValue, oldValue) => {
})
```
· watch可以监听多个响应式对线，任何一个响应式对象更新，就会执行回调函数
```shell
import { ref, watch } from 'vue' 
const count = ref(0) 
const count2 = ref(1) 
watch([count, count2], ([newCount, newCount2], [oldCount, oldCount2]) => { 
})
//还有第二种写法
watch([count, count2], (newValue, oldVlaue) => {
    console.log(newValue)//[newCount, newCount2]
    console.log(oldValue)//[oldCount, oldCount2]
})
```

## 状态和事件绑定
Vue 3.0 中定义状态的方法改为类似 React Hooks 的方法，下面我们在 Test.vue 中定义一个状态 count：
setup(){}函数内return 出的值可以使用this访问的到 这里面无法访问methods和data中的内容
点击事件@click=“click” 获取ref的时候和vue2.0一样 ref=‘refs’

```shell
<template>
  <div class="test">
    <h1 :ref='countOne'>test count: {{count}}</h1>
  </div>
</template>

<script>
  import { ref } from 'vue'

  export default {
    setup () {
// 设置默认值位0
      const count = ref(0)
      return {
        count
      }
    },
    methods:{
        countOne(el){
            // 组件返回的是一个Proxy 可以直接点后面跟着方法即可 
            // 非组件返回dom
            console.log(el)
            // 直接点可以直接调出来即可    
            console.log(el.message)
        }
    }
  }
</script>
```

## Vue 3.0 中初始化状态通过 setup 方法， 
组件也可使用
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

这里的 add(点击动作的方法) 方法不再需要定义在 methods 中，这样会造成setup内代码复杂化，找不到那个
但注意更新 count 值的时候不能直接使用 count++，而应使用 count.value++，
更新代码后，点击按钮，count 的值就会更新了：
在 methods 中不需要count.value++ 直接 count++ 即可

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
    // 定义一个url为空，开发环境为‘/api’，非开发环境为线上地址
    let apiurl = ''
    // 判断环境
    if(process.env.NODE_ENV === 'development'){
        apiurl = '/api';
    }else {
        apiurl = 'http://www.baidu.com/api'
    }
    // 1.1.创建axios的实例
    const instance = axios.create({
        baseURL: apiurl,
    })

    // 2.axios的拦截器
    // 2.1.请求拦截的作用
    instance.interceptors.request.use(config => {
        /**
        *  在这里写请求时候的token，等添加到请求之前的信息
        * 在此之前可以通过路由导航首位获取token，之后根据vuex来去添加token
        * 也可以保存到浏览器内，在浏览器内获取
        */
        console.log("请求拦截器")
        return config
    },err => {
        return err.data
    })

    // 2.2.响应拦截
    instance.interceptors.response.use(res => {
        console.log("响应拦截")
        /**
        * 返回来的数据，由于返回数据很多，截取code和data重要输出数据返回到页面进行渲染
        **/
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
```
组件中使用
```js
 const $api = require('@/api/ceshi.js')
 $api.getHomeMultidata().then(res=>{
   message.value = res.data
 })
```
开放api接口 需要建一个api.js文件，之后把全部的api接口导入进去，之后在min.js进行导入，之后开放这个api接口
```shell
// 导入api内容
import api from './api/api'
// 开放api接口
Vue.prototype.$api = api
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
    ceshi(){
      console.log('我是一直存在的，在这获取token和用户信息')
    }
  },
  modules: {
  }
})
```
Vuex 的语法和 API 基本没有改变,我们在 state 中创建了一个 test.a 状态，
在 mutations 中添加了修改 state.test.a 状态的方法： setTestA
actions s是调用就执行的一个，可以在router的导航守卫的时候进入之前巧用，
期间在这获取token和用户信息，之后保存到state中，这样即可保证刷新不会没有数据等
使用方法：store.dispatch('ceshi')（需要先导入 imputed store from ‘vuex的路径’）


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
      // 更改vuex的值：ctx.$store.state.test.a
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
这里我们点击 update a 按钮后，会触发 update 方法，此时会通过 ctx.$store.commit 调用 setTestA 方法，
将 count 的值覆盖 state.test.a 的值

## render函数
新建一个js文件之后直接写js即可
```shell
// jsx 写法，很多需要自己去查写法，比如文字在div中显示不出来，在span标签中显示出来
export default {
    data(){
        return {
            red:'red',
            cont:1
        }
    },
    render() {
let text = `<div class="works-wrapper" class="${this.red}">
                            <span>Hello</span>
                        </div>`
        return text
    },
    methods: {
    },
}
```
### 使用方法
```shell
import 命名的名称 from "@/components/render的js文件.js";
```

### 在render中使用
在模板中导入直接使用 - 和模板一个使用方式

## 遇到的问题
### 背景图片问题
如果背景在div的style标签上显示不出来的时候，需要添加 require('图片的路径')，下面由魔法字符串组成
 ```shell
<div class="machine-bg" :style="{
      backgroundImage:`url(${require('../common/sprite-map.jpg')})`
    }">
    </div>
```
### 如何让使用ref
在vue3.0之后的是 :ref='fangfa';这个是在方法内获取 
需要区别，第一个不带：是点页面加载没有直接打印出来的，
:ref='' 是页面加载的时候同时加载了
 ```shell
<div class="machine-bg" ref='refss' @click='click' :ref='fangfa'>
    </div>
    methods: {
        click(){
            console.log(this.$refs['refss'])
        },
        fangfa(el){
            console.log(el)
        }
    },
```
### 组件上使用v-model
默认情况下，v-model在组件上modelValue用作道具和update:modelValue事件。
我们可以通过将参数传递给来修改这些名称v-model：
监听组件内v-model 的input属性title值的变化
```html
<my-component v-model:title="bookTitle"></my-component>
```
在这种情况下，子组件将期望一个title prop并发出update:title事件以进行同步
update是input更新事件，这个为标识
```js
app.component('my-component', {
  props: {
    title: String
  },
  emits: ['update:title'],
  template: `
    <input
      type="text"
      :value="title"
      @input="$emit('update:title', $event.target.value)">
  `
})
```
```html
<my-component v-model:title="bookTitle"></my-component>
```

通过利用我们以前通过v-model参数来针对特定道具和事件的能力，
我们现在可以在单个组件实例上创建多个v-model绑定。

每个v模型将同步到不同的prop，而无需在组件中使用其他选项：
```shell
 // 使用
<user-name
  v-model:first-name="firstName"
  v-model:last-name="lastName"
></user-name>
 // 组件
app.component('user-name', {
  props: {
    firstName: String,
    lastName: String
  },
  emits: ['update:firstName', 'update:lastName'],
  template: `
    <input
      type="text"
      :value="firstName"
      @input="$emit('update:firstName', $event.target.value)">

    <input
      type="text"
      :value="lastName"
      @input="$emit('update:lastName', $event.target.value)">
  `
})
```
### 全局依赖注入 对所有子组件作用
title：vue的依赖注入provide，inject
注入
```shell script
  provide: {
    user: 'John Doe'
  },
```
使用：
```shell script
inject: ['user'],
```

Tags: 全局依赖注入
### 全局挂在的 通过min实现挂在
类型： [key: string]: any
默认： undefined
用法：
```js
app.config.globalProperties.foo = 'bar'
// 使用

app.component('child-component', {
  mounted() {
    console.log(this.foo) // 'bar'
  }
})
```
添加可以在应用程序内的任何组件实例中访问的全局属性。当键冲突时，组件的属性将具有优先权。
这可以代替Vue 2.xVue.prototype扩展：
```js
// vue2.0
Vue.prototype.$http = () => {}

// After vue3.0
const app = createApp({})
app.config.globalProperties.$http = () => {}
```
// Before
### 子组件继承父组件
自我理解：当父组件传一个参数给子组件，并且子组件上没有pros接收，应该
是默认到第一个父元素上去，需要改变元素的时候用到<main v-bind="$attrs">...</main>
v-bind="$attrs"进行接收
```shell 

```
### 事件传输
像组件和道具一样，事件名称提供了自动的大小写转换。如果在驼峰情况下从子组件发出事件，则可以在父组件中添加kebab大小的侦听器：

```shell script
this.$emit('myEvent')
<my-component @my-event="doSomething"></my-component>

```

可在js内定义一个emits: ['inFocus', 'submit']// 数组内是向外提供的事件名称
```shell script
app.component('custom-form', {
  emits: ['inFocus', 'submit']
})
```
与props外壳一样，我们建议您在使用in-DOM模板时使用kebab-cased事件监听器。如果您使用的是字符串模板，则此限制不适用。


