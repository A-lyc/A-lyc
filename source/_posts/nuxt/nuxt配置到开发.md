---
uuid: 16b19cb6-d44e-c99a-bb98-541698cd3a54
title: nuxt配置到开发
date: 2020-07-05 09:07:59
tags: nuxt配置到开发
category: Nuxt
---
中途的bug 一定熟练使用element有很大的帮助
0：上传服务器运行 - 服务器有缓存问题，把自身的缓存关闭，之后开启服务端缓存，期间有很多bug
1：状态存储到vuex中会使网页分享的时候带有用户的token，所以这个token需要在浏览器中获取之后发送网络请求，不要在vuex直接获取发送请求，会有bug
2：vuex有的时候进入页面点击刷新的时候很多东西没有了，因为存储的状态管理没有了，所以导致显示或者不显示的问题，等等，如果用户的话，这个还是判断浏览器中的token有没有，在使用浏览器的token获取用户信息，之后在进行判断，以防刷新之后不显示问题
3：页面数据加载不出来：因为没有正确使用asyncData数据请求，或者axios写错了位置，应该在插件目录下
4：vuex中nuxtServerInit的使用， mutations内使用大写TOKEN(){},之操作一件事情
5：nuxt使用node_mode中的axios的时候会报错官方解释是，所以尽量使用自己带的axios
如果您的项目中直接使用了node_modules中的axios，并且使用axios.interceptors添加拦截器对请求或响应数据进行了处理，确保使用 axios.create创建实例后再使用。否则多次刷新页面请求服务器，服务端渲染会重复添加拦截器，导致数据处理错误。
<!-- more -->
##  一、为什么要用Nuxt.js
原因其实不用多说，就是利用Nuxt.js的服务端渲染能力来解决Vue项目的SEO问题。
####    1：安装
npx create-nuxt-app <project-name>
cd <project-name>
进入环境命令：
npm run dev
打包编译
npm run dev
之后生成 dist（vue）和.nuxt（服务器:如果用中间件需要把中间件的文件一起打包发给后端，基本文件：.nuxt,state,nuxt.confige.js,package.json）
npm i 之后 npm start - 项目可以跑起来了

### 2配置 - 见nuxt的使用
