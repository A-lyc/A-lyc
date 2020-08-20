---
title: nuxt跳转手机端页面
date: 2020-08-20 14:41:34
tags: nuxt跳转手机端页面
category: nuxt跳转手机端页面
---
新建一个isMobile.js文件，路径自选，我的路径是/static/js/isMobile.js
```js
(function() {
    console.log("判断移动端");
    var sUserAgent = navigator.userAgent.toLowerCase();
    if (/ipad|iphone|midp|rv:1.2.3.4|ucweb|android|windows ce|windows mobile/.test(sUserAgent)) {
        console.log("移动端");
        //跳转移动端页面
        window.location.replace("xxxx");//跳转后没有后退功能 
    } else {
        console.log("PC端");
    }
})();
```

在nuxt.config.js加上对应配置
```json
head: {
    script:[  
      {src:'js/isMobile.js'},
    ]
  },
```

重新启动项目可以看到效果