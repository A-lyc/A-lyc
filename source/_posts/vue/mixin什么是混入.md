---
uuid: 7bdf5bd1-ecac-fe95-a36b-e0a1f607d71f
title: mixin什么是混入
date: 2020-03-28 18:09:51
tags: vue mixin混入
category: vue
---

就是在一个公共的js中可以直接引入梦里面可以定义data，metahds，components，directives等，直接添加到相应的方法内。metahds中会添加子级，但是重名的时候会直接替换
<!-- more -->
生命周期是可以的
有冲突的时候以组件内的优先
在外面定义一个js文件如mixin.js
导出文件
```
export const timeListenerMiXin//导出名称 = {
data(){}
}
```
引入 
```
import {timeListenerMiXin} from '@/common/mixin';
```
实例中调用 :   
```
name: "Detail",
mixins:[timeListenerMiXin],
```
之后MiXin里面的timeListenerMiXin内的组件条件（data，popst等）会应用到导入的组件中，直接使用this可以调用
