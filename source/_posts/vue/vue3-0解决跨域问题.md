---
uuid: 9d3915ea-0ad1-a049-ce3c-c16dd67eae3b
title: vue3.0解决跨域问题
date: 2020-03-31 18:22:37
tags:[ vue,跨域]
category: vue
---

在vue的vue.config.js（自己创建的）里面进行添加

```js
module.exports = {
    devServer: {
      proxy: {
        '/api': {
          target: 'https://www.XXXXXXXXX.com/',
          changeOrigin: true,
          ws: true,
          pathRewrite: {
            '^/api': ''
          }
        }
      }
    }
}

```

使用的时候，发送网络请求：需要/api/请求地址

自己封装的axios：
```js
import axios from 'axios'

export function request(config) {
  // 1.创建axios的实例
  const instance = axios.create({
    '/api': 'https://www.XXXXXXXXX.com/',
  })

  // 2.axios的拦截器
  // 2.1.请求拦截的作用
  instance.interceptors.request.use(config => {
    return config
  },err => {
    return err.data
  })

  // 2.2.响应拦截
  instance.interceptors.response.use(res => {
    return res.data
  },err => {
    return err.data
  })

  // 3.发送真正的网络请求
  return instance(config)
}

```

调用网络请求：

```
import {request} from "./request";

//前面的api是地址必加
export function getCeshi(){
  return request({
    url: '/api/XXX.php',
  })
}
export function getArea(city,area){
  return request({
    url: '/api/XXX.php',
    params:{
      city,
      area
    }
  })
}
```