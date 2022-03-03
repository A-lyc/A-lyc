---
title: egg后台开发
date: 2022-02-16 18:32:25
tags: egg后台开发
category: egg后台开发
---
# 安装node，之后安装egg
   · 自己已写完一个egg，基础的增删改查，基础上写了一个后台，可在这个基础之上继续开发，对接数据库的时候很不友好。需要改进和数据库的链接

# egg目录介绍
 主要文件：app，config，
 app
   - router.js
   撰写路由，数据在controller这里来 1：请求的名称，2：中间件，下面这个是判断有无token，3：controller文件夹下news.js内的list方法
   router.get('/news/list', app.middleware.checktoken(), controller.news.list);
   
