---
uuid: 833f0485-8d52-3438-d615-3a6ce5461948
title: vue解决跨域问题
date: 2020-03-28 18:02:07
tags: vue
category: vue
---
```
vue.config.js
	// https://www.XXXXX.com

module.exports = {
  publicPath: './',//解决打包不显示页面（页面空白） 问题因为路径不对
  devServer: {
    proxy: {
      '/api': {
        target: 'https://www.XXXX.com',
        changeOrigin: true,
        ws: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    }
  },
}
修改axios实例
	  // 1.创建axios的实例
  const instance = axios.create({
    '/api': 'https://www.XXX.com/',
  })
应用的时候，注意api
	export function getCeshi(){
  return request({
    url: 'XXX',
  })
}
```