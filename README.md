### 本地进入环境查看
``
hexo server
------------

### 添加文章
``
hexo new '文章名'
---------------

### 清理缓存
``
hexo clean
清除缓存文件 (db.json) 和已生成的静态文件 (public)
------------

###  文章分类需要添加标签
``
--category: 分类标签
------------

### 添加图片
--<img src=""> 不可点击放大
--![查入图片](/02.png) 可点击放大
-------------

### 创建一个分类，分类下有文章
``
--hexo new page --path about/me - 必须格式
此时 Hexo 会创建 source/_posts/about/me.md，
同时 me.md 的 Front Matter 中的 title 为 "page"。这是因为在上述命令中，
hexo-cli 将 page 视为指定文章的标题、并采用默认的 layout。
-------------------

--  比如说新建标签页面，执行命令之后会在根目录source文件夹下创建tags文件夹
hexo new page "tags"
------------------


### 创建文件夹
``
hexo new page --path tagcloud/index 标签云
-----------------------

### 去除表单内格式的html
``
进环境的时候注意看这个（去除表单内格式的html），会出现很多无用代码
---------

### npm 卸载
``
npm uninstall 插件名
--------------

在你的文章的front-matter中提供缩略图的路径或URL地址：
post.md
title: Icarus快速上手
thumbnail: /gallery/thumbnails/desert.jpg
---
文章内容...
