---
uuid: 7432f33f-0c21-8a81-a0af-ae199539e93c
title: vue-cli3.0脚手架搭建项目vant
date: 2020-04-02 14:35:44
tags: vue
category: vue
---
*   node和npm安装完成的情况下，安装vue的3.0脚手架项目，npm i -g @vue/cli ，之后创建项目vue create hello word（项目名称叫hello word），

##  有icon的话可以引入
// 引入iconfont的样式
import './assets/iconfont/iconfont.css'

##  安装vant（ui框架）
npm i vant -S

##  引入组件
*   安装插件
npm i babel-plugin-import -D

*   在babel.config.js配置

```
module.exports = {
  presets: [
    '@vue/app'
  ],
  plugins: [
    ['import', {
      libraryName: 'vant',
      libraryDirectory: 'es',
      style: true
    }, 'vant']
  ]
}
```

*   在src/main.js配置

```
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

// 引入Vant UI组件的样式
import 'vant/lib/index.css'

// 1.引入axios
import axios from 'axios'

Vue.config.productionTip = false

// 2.把axios绑定到vue实例的属性$axios
Vue.prototype.$axios = axios

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')

```

##  测试效果

```
<template>
  <div class="home">
    home
    <div class="iconfont icon-iconfonthome0"></div>
    <van-button type="primary">主要按钮</van-button>
  </div>
</template>

<script>
import Vue from "vue";
import { Button } from "vant";
Vue.use(Button);
export default {
  name: "home",
  mounted() {

      //使用axios
    this.$axios
      .get("https://www.xxx.com")
      .then(res => {
        console.log(res);
      });
  }
};
</script>
<style lang="less" scoped>
</style>
```