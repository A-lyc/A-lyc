---
uuid: 915c0da2-50bc-2e76-d210-c6d0830a5543
title: uniapp批量导入组件
date: 2020-05-19 23:38:17
tags: uniapp
category: uniapp
---
uni-app easycom 模式 组件 引入
前言 （报错的问题 请往下看）
easycom官网描述
![查入图片](/01.png)
含义：
原本 vue 引入组件 需要多部操作，全局引入、单文件引入、在文件中注册等等等 非常麻烦。

uni-app 推出了easycom 引入组件模式 只需要在page.json 中配置自动扫描 或 自定义规则导入组件即可下面我会详细说下两种用法。

esaycom 自动导入：
一般在组件目录下（components） 目录下按照他的规则 组件文件夹/组件名称.vue 就可以自动导入了 如下图所示 ↓↓↓ 。
![查入图片](/02.png)

esaycom 手动编写规则导入：
一般报错的位置都在这里 写了自动以规则但是无法使用 ，找不到组件 ，非常头疼。
下面是一个示例 请看下方图片 ↓↓↓。
![查入图片](/03.png)

这里官网写的示例 让这么引入
在这里插入图片描述
问题 如果我在多加一个文件夹呢？ 因为全部的组件都这么放的话，问题会很大。因为组件越来越多，严重影响代码质量，美化等等。

如果把组件的作用不同 单分文件夹的时候 这个 自定义规则应该怎么写呢？ 看下图所示↓↓↓↓。
![查入图片](/04.png)

把组件单分到文件夹里面。
在这里插入图片描述
那么这个规则就应该这么写。
![查入图片](/05.png)

```
"easycom": {
  "autoscan": true,
  "custom": {
    "uni-(.*)": "@/components/uni-$1.vue", // 匹配components目录内的vue文件
    "vue-file-(.*)": "packageName/path/to/vue-file-$1.vue" // 匹配node_modules内的vue文件
  }
}
```
### 自定义easycom配置的示例
如果需要匹配node_modules内的vue文件，需要使用packageName/path/to/vue-file-$1.vue形式的匹配规则，其中packageName为安装的包名，/path/to/vue-file-$1.vue为vue文件在包内的路径。


### 说明
easycom方式引入的组件无需在页面内import，也不需要在components内声明，即可在任意页面使用
easycom方式引入组件不是全局引入，而是局部引入。例如在H5端只有加载相应页面才会加载使用的组件
在组件名完全一致的情况下，easycom引入的优先级低于手动引入（区分连字符形式与驼峰形式）
考虑到编译速度，直接在pages.json内修改easycom不会触发重新编译，需要改动页面内容触发。
easycom只处理vue组件，不处理小程序组件。暂不处理后缀为.nvue的组件，建议参考uni ui，使用vue后缀，同时兼容nvue页面。
nvue页面里的.vue后缀的组件，同样支持easycom
tabBar