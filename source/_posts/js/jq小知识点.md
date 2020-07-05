---
uuid: 1680ee82-95cb-9e41-c4b5-a21bde1954ce
title: jq小知识点
date: 2020-04-03 13:28:32
tags: jq
category: ES6
---

//找到元素
let $header = $('.comp-menu')

//全局找到命名这个的元素[data-open="menu"]
let $header = $('[data-open="menu"]')
<!-- more -->
//执行事件
$header.on('click', function () {})

//动态添加css样式
$header.css('opacity','1')

//可以利用添加删除类来实现动画，动画内不要使用display：none
//添加类
$headerEl.addClass('active')

//删除类
$headerEl.removeClass('active')

//获取卷曲距离
$(window).on('scroll', function () {
    
    //获取Y轴卷曲的距离
    console.log(window.pageYOffset)
})

