---
uuid: fdb622e5-fb06-d84b-e46e-3b995d94f1f2
title: vue中的slot-scope=scope
date: 2020-04-19 21:35:22
tags: vue
category: vue
---
在vue中使用插槽,有匿名插槽,具名插槽,还有一个具有数据的插槽,就是说可以读取插槽上传来的数据,和实例data上的数据不会冲突
匿名插槽:<slot></slot>,没有命名的外部直接使用,定义一个子组件可以直接向插槽内输入内容
具名插槽:<slot name="name"></slot>,具名插槽,外部需要<div slot="name"></idv>使用
数据插槽:
组件定义插槽
```
<template>
    <div>
        <div>下面是一个slot</div>
        <slot a="123" b="msg" ></slot>
    </div>
</template>
```
使用:slot-scope="scope"
```
<div>
	<mysolt>
	<template slot-scope="scope">
    <div>{{scope}}</div>// a="123" b="456"
    <div>{{scope.a}}</div>// 123
    <div>{{scope.b}}</div>// 456
  </template>
	</mysolt>
</div>
```