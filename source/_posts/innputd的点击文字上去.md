---
uuid: 5cd0bb34-2e6a-1994-fdaf-8928af425a25
title: innputd的点击文字上去
date: 2020-03-23 09:50:56
tags: 前端脚手架
category: 前端模块化脚手架
---
给input一个相对定位之后使用valid属性，同级别的p标签

```
<div class="from-content" id="from">
    <div class="from-group">
       <input class="input-click" required>
      <p class="from-input-p">Company</p>
    </div>
 </div>
```
input在上，文字在下
给input一个相对定位之后使用valid属性，同级别的p标签
```
Input：vaild+p{//给p设置样式
一个动画即可
}
input {
       position: relative;
       z-index: 20;
       background: transparent;
       }
p {
       position: absolute;
       top: 0;left: 0;
       transform-origin: left center;
       }
input:valid + p {
        transform: scale(0.8) translateY(-30px);
       }
```