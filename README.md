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
--<img src="">
--![查入图片](/02.png)
-------------

### 创建一个分类，分类下有文章
``
--hexo new page --path about/me
此时 Hexo 会创建 source/_posts/about/me.md，同时 me.md 的 Front Matter 中的 title 为 "page"。这是因为在上述命令中，hexo-cli 将 page 视为指定文章的标题、并采用默认的 layout。
-------------------


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