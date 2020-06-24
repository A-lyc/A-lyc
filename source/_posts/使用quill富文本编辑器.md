---
uuid: cd0cb1c4-cd01-9157-3a73-e1289d138cf5
title: 使用quill富文本编辑器
date: 2020-06-23 20:15:32
tags: vue nuxt
category: vue nuxt
---
##  首先安装 quill富文本编辑器 使用npm安装
npm install quill@1.3.4 或者
npm install vue-quill-editor --save
##  cdn安装
```html
<!-- Quill主要库 -->
<script src="//cdn.quilljs.com/1.3.4/quill.js"></script>
<script src="//cdn.quilljs.com/1.3.4/quill.min.js"></script>

<!-- 主题样式 -->
<link href="//cdn.quilljs.com/1.3.4/quill.snow.css" rel="stylesheet">
<link href="//cdn.quilljs.com/1.3.4/quill.bubble.css" rel="stylesheet">

<!-- 核心库，不包含主题、格式化等非必要模块 -->
<link href="//cdn.quilljs.com/1.3.4/quill.core.css" rel="stylesheet">
<script src="//cdn.quilljs.com/1.3.4/quill.core.js"></script>
```
##  使用npm安装的时候
```js
//min.js 全局安装插件
import VueQuillEditor from 'vue-quill-editor/dist/ssr'
Vue.use(VueQuillEditor);
// quillEditor
import { quillEditor } from "vue-quill-editor";
//css： 
import "quill/dist/quill.core.css";
import "quill/dist/quill.snow.css";
import "quill/dist/quill.bubble.css";

```
