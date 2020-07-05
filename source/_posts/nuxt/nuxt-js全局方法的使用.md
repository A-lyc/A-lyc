---
uuid: ff8a1f3a-d14e-3956-8253-e056eeb8d450
title: nuxt.js全局方法的使用
date: 2020-06-20 22:03:02
tags: nuxt.js全局方法的使用 install
category: Nuxt
---
nuxt.js全局方法的使用 见vue插件使用的：https://cn.vuejs.org/v2/guide/plugins.html
1.在plugins的文件夹地下去建立一个proto.js的文件
<!-- more -->
```
import Vue from 'vue'
var test = {
    install(Vue) {
        Vue.prototype.test = {
            val: function(val) {
                console.log('打印出来的值');
            }
        };
        Vue.prototype.testname = '四叶草'
    }
}
Vue.use(test)
```
2.直接使用
```
 created() {
    console.log(this.$route.params, 'this.$route');  
    console.log(this.testname, 'this'); //全局去拿字符串
  },
  mounted() {
    this.test.val()  //全局去调用事件
  }
  ```
在created里面去调用事件会报错