---
uuid: bea548e6-eb93-0eb2-d70a-a0b89753cfdd
title: 'Vue CLI'
date: 2020-03-28 21:00:25
tags: vue
category: vue
---

Vue.js官方脚手架工具就使用了webpack模板
首先安装好node和git，npm（安装node会一起安装）

对所有的资源会压缩等优化操作
它在开发过程中提供了一套完整的功能，能够使得我们开发过程中变得高效。

Webpack的全局安装
  npm install webpack -g


安装Vue脚手架
  npm install -g @vue/cli

Vue CLI2初始化项目
  vue init webpack my-project

Vue CLI3初始化项目
  vue create my-projec

vue-cli2
![查入图片](/vue-cli.png)
![查入图片](/vue-cli-2.png)
简单总结
如果在之后的开发中，你依然使用template，就需要选择Runtime-Compiler
如果你之后的开发中，使用的是.vue文件夹开发，那么可以选择Runtime-only

vue-cli 3 与 2 版本有很大区别
vue-cli 3 是基于 webpack 4 打造，vue-cli 2 还是 webapck 3
vue-cli 3 的设计原则是“0配置”，移除的配置文件根目录下的，build和config等目录
vue-cli 3 提供了 vue ui 命令，提供了可视化配置，更加人性化
移除了static文件夹，新增了public文件夹，并且index.html移动到public中
![查入图片](/vue-cli-3.png)
![查入图片](/vue-cli-4.png)
UI方面的配置
启动配置服务器：vue ui

vue.config.js
```
module.exports = {
  publicPath: './',
  configureWebpack: {
    resolve: {
      alias: {
        'assets': '@/assets',
        'common': '@/common',
        'components': '@/components',
        'network': '@/network',
        'views': '@/views',
      }
    }
  },
  //如有跨域问题：见vue3-0解决跨域问题
}
```
安装axios，异步：详情见：vue的axios封装好的
需要安装 npm i axios --save

待续，待更新
