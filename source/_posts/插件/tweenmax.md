---
title: tweenmax
date: 2021-09-26 18:32:25
tags: [tweenmax,抽奖,动画]
category: tweenmax
---
# gsap NPM / 构建工具 下载安装，TweenMax的插件
```shell
npm i --save-dev gsap
import TweenMax from 'gsap'
```
## 默认使用ES 模块。您可以导入单个类，例如：
```js
import { gsap } from "gsap";
import { PixiPlugin } from "gsap/PixiPlugin.js";
import { MotionPathPlugin } from "gsap/MotionPathPlugin.js";
gsap.registerPlugin(PixiPlugin, MotionPathPlugin);
```
## 如果您使用服务器端渲染，您可能需要在您的配置设置（如 nuxt.config）中将 GSAP 添加到您的 transpile 属性中：

```js
build: {
    ...
    transpile: ['gsap'],
},
```



具体使用看老虎机插件

# 初始化基本操作
```js
var tl = new TimelineLite();
tl.add( TweenLite.to(element, 1, {left:100}) );//将一个动画添加到时间轴
tl.add( TweenLite.to(element, 1, {top:50}) );//将一个动画添加到时间轴末端，即与前一个动画接续
tl.add( TweenLite.to(element, 1, {opacity:0}) ); //将一个动画添加到时间轴末端，即与前一个动画接续
 
//控制时间轴
tl.pause();
tl.resume();
tl.seek(1.5);
tl.reverse();

// 使用简单的to()方法和链式调用使其更加简洁：
var tl = new TimelineLite();
tl.to(element, 1, {left:100}).to(element, 1, {top:50}).to(element, 1, {opacity:0});
```

文档网址可能会打不开 需要添加.html后缀：
https://www.tweenmax.com.cn
https://greensock.com/docs/v3/GSAP/Tween
