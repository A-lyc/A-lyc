---
uuid: f56a4a52-8d2e-c4fc-efa0-1069de954ea8
title: js找不到this问题
date: 2020-03-30 11:29:37
tags:
category:
---
>   在函数中找不到this的问题，可以重新定义一个this指向，that指向最外层data中this的this

```
async selectGuwen (project_id) {
        var that = this
        that.showGuwen = true
        that.list_id = project_id
        uni.request({
          url: '',
          data: {
            action: 'adviser',
            dopost: 'adviser_list',
            openid:this.openId,
            project_id:project_id
          },
          method: "post",
          header: {
            'content-type': 'application/x-www-form-urlencoded'
          },
          success: function (data) {
            console.log(data)
<!-- this.guwenData = data.data.data -->
/*
*//这个位置的this是找不到的，返回undefind//指向的是这个父级函数中的data
*找的是async selectGuwen 函数内的guwenData，
*所以在async selectGuwen 重定义一下this，去找data中的this
*/
            that.guwenData = data.data.data//
          },
          error: function (data) {
            console.log(data)
          }
        })
      },
```