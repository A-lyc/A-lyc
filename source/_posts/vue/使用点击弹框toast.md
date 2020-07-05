---
uuid: 4adf9ad9-0bdf-9398-3064-f86102b5ff85
title: 使用点击弹框toast
date: 2020-03-28 17:09:49
tags: vue插件 弹窗 toast
category: vue
---
vue自定义的插件点击弹框效果

min.js添加一个自定义插件
<!-- more -->
``
//自定义的插件
import toast from '@/components/common/toast'
Vue.use(toast)
``

使用

```
//参数为显示能内容和时间
this.$toast.isShow('res',2000)
```