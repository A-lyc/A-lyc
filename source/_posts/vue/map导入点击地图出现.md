---
uuid: be905f4a-5bef-cf9d-5ef6-928bbcb6f84b
title: map导入点击地图出现
date: 2020-03-23 09:54:48
tags: [前端脚手架]
category: 前端模块化脚手架
---
需要引入js
npm i --save jason-webmap
安装，使用秦哥脚手架自动安装的
然后建立一个wenmap的文件，
<!-- more -->
webmap-Js：
```
import webmap from 'jason-webmap';
import '~jason-webmap/dist/jason-webmap.css';
// or// 推荐，可通过变量定制
import '~jason-webmap/src/style.scss';
webmap({
  // 激活 map 的按钮选择器  选择器是根据a标签的open-map来的，可以写成a[open-map=”webmap”]
  openSelector: 'a[open-map=webmap]',
  // 跟随者 map 一同移动的内容选择器  最好市body市第一父级，之后有个自己名称叫做comp-root类名
  moveSelector: '.comp-root'
})
```

webmap-ejs
```
<aside class="jason-map">
  <div class="jason-map-content">
    <h3 class="jason-map-heading">网站导航</h3>
    <div>
      <div class="jason-map-item">
        <h4 class="jason-map-title"><a href="#">公司简介</a></h4>
        <ul class="jason-map-ilist">
          <li><a href="#">导航</a></li>
          <li><a href="#">导航</a></li>
          <li><a href="#">导航</a></li>
          <li><a href="#">导航</a></li>
          <li><a href="#">导航</a></li>
        </ul>
      </div>
      <div class="jason-map-item">
        <h4 class="jason-map-title"><a href="#">公司简介</a></h4>
        <ul class="jason-map-ilist">
          <li><a href="#">导航</a></li>
          <li><a href="#">导航</a></li>
          <li><a href="#">导航</a></li>
          <li><a href="#">导航</a></li>
          <li><a href="#">导航</a></li>
        </ul>
      </div>
    </div>
    <div class="jason-map-footer">
      <h5>版权所有</h5>
      <h5>xxxxxxx 有限公司 </h5>
      <p class="mt-xs-10">鲁ICP备 xxxxxxx 号</p>
      <p>网站设计：jason</p>
    </div>
  </div>
</aside>
```

webmap - Css：
```
@import "../../assets/styles/utils";
@import '~jason-webmap/src/style.scss';
```
需要导入css
定制的时候可以使用强制执行来做

footer组件-ejs
```
  <a href="#" open-map="webmap">
    DESIGNED BY LTD
  </a>
```


首页以及其他页面 - ejs  和内容为兄弟级别
```
<body>
 <main class="comp-root"></main>
<%= require('../../components/webmap/index.ejs')() %>
</body>
```

语法引入：
```
<%= require('../../components/webmap/index.ejs')() %>//创建一个webmap组件文件进行引入，

```
