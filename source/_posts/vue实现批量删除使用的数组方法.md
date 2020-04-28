---
uuid: 36d16d95-548c-7a72-0955-e9e1ac93f138
title: vue实现批量删除使用的数组方法
date: 2020-04-28 18:01:51
tags: vue
category: vue
---

定义一个方法，之后取到全部的数组，循环遍历使用forEch方法找到有没有符合条件的，（没有返回-1），之后使用findIndedx方法遍历和原数组进行对比，有的话返回一个原数组的索引值，这个索引值有的话forEach这个的值不会返回-1，之后判断，有这个索引进行删除，没有跳过
```js
    deleteAll(){
      let arr = this.multipleSelection.forEach(res => {
       let index = this.tableData.findIndex(item => {
          item.id === res.id
        })//返回数组的索引值
        if(index !== -1){
          this.tableData.splice(index,1)
        }
      })
        this.multipleSelection = [] //情况循环中的数组
      console.log(arr)
    },

```