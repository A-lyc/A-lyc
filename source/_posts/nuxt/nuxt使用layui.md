---
title: nuxt使用layui
date: 2020-09-05 22:46:34
tags: nuxt使用layui
category: nuxt使用layui
---
首先下载layui，在码云https://gitee.com/sentsin/layui或者githuphttps://github.com/sentsin/layui/上下载

之后提取下载文件内的dist文件赋值出来放置到公共文件夹static这个内dist全部目录

之后在nuxt.confing.js导入一下
```js
script:[
      {src:'/layui/layui.js'},
    ],
```
在使用到的组件内导入css
```css
@import "static/layui/css/layui.css";
/*//引用线上的css*/
@import "https://cdn.jsdelivr.net/npm/bootstrap@4.5.0/dist/css/bootstrap.min.css";
```
之后在mounted函数内使用
```js
mounted() {
      layui.use('layedit', function(){
        var layedit = layui.layedit;
        layedit.build('demo'); //建立编辑器
      });
    }
```