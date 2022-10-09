---
title: uniapp manifest.json
date: 2022-10-09 11:51:04
tags: [manifest.json,uniapp,配置]
category: uniapp云开发
---

## 关闭一打开页面的一个提示问题
```json
"compatible" : {
    "ignoreVersion" : true //true表示忽略版本检查提示框，HBuilderX1.9.0及以上版本支持  
},
```