---
uuid: fc3a8894-06f8-d5e3-646f-0c125d629961
title: vue插件懒加载和移动端延迟
date: 2020-03-25 20:27:58
tags: vue插件
category: vue
---

> 在移动端会使用fastClick插件
* 安装npm i fastclick
  
  导入：min.js中：import FastClick from 'fastclick'
  
  调用：//解决移动端的300ms延迟
  FastClick.attach(document.body)

---------------

> 图片懒加载
* 安装npm i vue-lazyload -save 
  
  导入:min.js:
  import VuelazyLoad from 'vue-lazyload'
  //使用图片懒加载插件
  Vue.use(VuelazyLoad)

  使用:
  img :src=“./img.jpg”修改成v-lazy=“./img.jpg”