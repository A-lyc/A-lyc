---
uuid: 2b4490bb-2bee-1db6-43c0-6616792dffc4
title: 微信使用Vant-weapp
date: 2020-05-15 21:18:25
tags: 微信 Vant-weapp
category: 微信
---
### 微信小程序引入Vant组件库
Vant Weapp组件库：https://youzan.github.io/vant-weapp/#/intro
<!-- more -->
说说我在引入vant组件库的时候的操作方法吧：

1.先在微信开发者工具中打开项目的终端：

2.然后初始化一个package.json文件：输入命令：npm init  => 一定是npm init

然后一路回车

项目就回产生一个package.json文件：

3.接着在vant组件库的官网上找到安装语句：npm i vant-weapp -S --production，在终端输入安装命令，点击回车：

官网安装：https://youzan.github.io/vant-weapp/#/quickstart

4.构建npm：在微信开发者工具的菜单栏中找到工具栏的选项“构建npm”，等待构建完成

![查入图片](/01.webp)

其中miniprogram_npm下就是vant-weapp组件库；

5.最后，在微信开发者工具的详情里面将“使用npm模块"勾选上，如下

![查入图片](/02.webp)

6.引用和使用vant组件：（关于如何引用和使用组件可以参考官方文档噢，很齐全）

以引用button按钮为例，官网文档中都写的特别详细了：

![查入图片](/03.webp)
![查入图片](/04.webp)

