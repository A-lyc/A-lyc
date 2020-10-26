---
title: node基本插件
date: 2020-10-19 21:35:13
tags: [node,node服务器，node插件]
category: node
---
## express node框架可查官网
- 官网：https://www.expressjs.com.cn/starter/installing.html
```shell
npm i --save express
```
## art-template 模板插件
- 官网：https://aui.github.io/art-template/zh-cn/index.html
```shell
npm install --save art-template express-art-template
```
## nodemon 修改i自动更新代码
- node自带应该 最好全局安装
```shell
npm i nodemon -g
```
## 解析post请求体的插件
```shell
npm i --save body-parser
```

## 在Node中操作mongod数据库
- 使用官方的mongod包来操作
 · 网址gethup：https://github.com/mongodb/node-mongodb-native

## 使用第三方mongoose框架来操作mongoDB操作数据库
 - 第三方包，基于mongoDB官方第三方包做了封装，名字交mongoose
   · 官网：http://www.mongoosejs.net/
 - 安装使用
```shell
npm i --save mongoose
```