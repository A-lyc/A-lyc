---
title: vue打包后的文件如何本地运行
date: 2023-09-19 20:07:03
tags: [vue,vue打包后的文件如何本地运行]
category: vue打包后的文件如何本地运行
---

- 如果您想在本地运行 Vue 打包后的文件，您需要安装一个 HTTP 服务器，并通过该服务器来加载打包后的文件。
  推荐使用 npm 包 http-server：

 1.  首先在您的 Vue 项目目录中安装 http-server：
```js
npm install -g http-server
```
 2. 然后将当前目录设置为您的 Vue 项目的打包文件夹，例如 dist：
```js
cd dist
```
 3. 最后，启动 HTTP 服务器：
```js
http-server
```

- 您可以通过浏览器访问 http://localhost:8080 来查看您的 Vue 项目。
- 在根目录 package.json 添加  之后npm run ser     ser:自己定义名称
```json
{
 "scripts": {
   "test": "http-server ./dist"
 }
}
```
