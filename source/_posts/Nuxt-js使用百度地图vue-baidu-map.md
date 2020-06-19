---
uuid: 57058550-2bb2-0556-34a3-b70883f0ee01
title: Nuxt.js使用百度地图vue-baidu-map
date: 2020-06-19 13:48:48
tags: Nuxt
category: Nuxt
---

##  vue-baidu-map文档：https://dafrok.github.io/vue-baidu-map/#/zh/start/usage

安装vue-baidu-map
```
npm i vue-baidu-map -D
```
在plugins新建map.js:
```
import BaiduMap from 'vue-baidu-map'
import Vue from 'vue'
Vue.use(BaiduMap, {
  ak: '申请的百度地图密匙'
})
```
在nuxt.config.js中引入：
```
  plugins: [
    { src: "~plugins/map.js", ssr: false },
  ],
```
在页面中使用：
```html
<template>
  <div>
    <baidu-map class="bdwindow" :dragging="dragging" :center="center" :zoom="zoom" style="height:500px" :scroll-wheel-zoom='scroll'>
          <bm-info-window :position="center" :title="title" :show="show">
              <p class="bdwtext" v-html="contents"></p>
          </bm-info-window>
    </baidu-map>
  </div>
</template>
<script>
export default {
  data () {
        return {
            jump_path:"",
            center: {lng: 120.4373010751, lat: 23.1095638170},
            zoom: 15,  //缩放级别
            title:"标题",
            contents: '地址：具体地址信息',  //标签内容
            show: true,  //显示标签
            scroll:true,  //地图缩放
            dragging:true,  //地图拖拽
        }
    }
}
</script>
```