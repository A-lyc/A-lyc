---
uuid: 0dd94664-6eaf-7821-00f7-759ba51d8789
title: vue点击那个那个变色
date: 2020-04-01 22:48:10
tags: vue
category: vue
---

利用class来实现，也可以使用style来实现。class实现时候是 :class='{ active:index===number}'>。style实现的时候：:style='{ color:index===number ? active:false}'>

```
<style>

.con{ 

        color: red;

    } 

</style>
```

```
<ul id="app">
<li v-for='(item,index) in items'    
@click="change(index)"   
:class='{ active:index===number}'<!--通过切换索引值改变class-->
>        
　　<span v-html="item.name"></span>
</li>
</ul>
```
```
data(){
  number: 0,    //注意此处
  items:[  1, 2, 3 ] 
},

```
```
methods: {   
change(index) {
this.number = index; //重要处
} 
} 

```