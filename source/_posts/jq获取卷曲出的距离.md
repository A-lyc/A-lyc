---
uuid: 2376cc08-f9b4-d46a-fad2-25d7a064b73b
title: jq获取卷曲出的距离
date: 2020-04-02 10:14:04
tags: 前端脚手架
category: 前端模块化脚手架
---
使用jq获取卷曲的距离，秦哥脚手架

```
let $headerEl = $('.comp-header')

//获取window的卷曲函数
$(window).on('scroll', function () {

//判断卷曲距离window.pageYOffset获取卷曲的距离
  if (window.pageYOffset !== 0) {

      //使用添加类删除类的方式
    $headerEl.addClass('active')
  } else {
    $headerEl.removeClass('active')
  }
})
```