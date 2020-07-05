---
uuid: d624d0e9-d695-5cb6-373f-74d27dfa0727
title: wxparse富文本插件
date: 2020-05-18 20:41:46
tags: 微信 wxparse富文本插件
category: 微信
---
优点：目前已知唯一可以转化HTML到小程序识别的插件

缺点：转换一个HTML标签可能需要大量的微信小程序标签还有样式
<!-- more -->
配置：第一步，下载https://github.com/icindy/wxParse

第二步，放入项目中，我选择pages目录下

第三步，配置

wxml加入：
```html
<import src="../wxParse/wxParse.wxml"/>
```
在需要的地方使用：
```
<template is="wxParse" data="{{wxParseData:article.nodes}}"/>
```

其中article是后台html值的变量名
js加入：
```js
var WxParse = require('../wxParse/wxParse.js');
//或者
import WxParse from '../wxParse/wxParse.js'

Page({
  data: {
    article:'<div>我是HTML代码</div><h4><i>我是h1标签</i></h4>'
  },
  //但是要注意的是a标签的转化，需要加入一个方法，示例如下
  wxParseTagATap: function (e) {
    var href = e.currentTarget.dataset.src;
    console.log(href);
    wx.redirectTo({
      url: href
    });
  },
  onLoad: function () {
    WxParse.wxParse('article', 'html', this.data.article, this, 5);
    }
})
```

这里貌似使用es6的import会有错误
我在onload事件写下了：
```js
WxParse.wxParse('article', 'html', this.data.article, this, 5);
```

注意的是第三个和第四个参数，前几个可以固定不变但是第一个要和数据变量名一致，第三个是后台数据，第四个是指的小程序标签，可以注册多个wxparse
wxss加入：
```css
@import '../wxParse/wxParse.wxss';
```

到此完成，但是要注意的是a标签的转化，需要加入一个方法，示例如下：
```js
wxParseTagATap: function (e) {
var href = e.currentTarget.dataset.src;
console.log(href);
wx.redirectTo({
url: href
});
}
```

这个在点击a标签的时候控制台其实是输出了警告信息的
此外url也只能是小程序内部地址，这是个限制，他不能跳到外部，这里我想后台编辑的时候可以用二维码替代，小程序跳转外部地址可以使用web-view标签，详情参考官方文档