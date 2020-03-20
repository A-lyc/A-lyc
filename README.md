### 本地进入环境查看
``
hexo server
------------

### 添加文章
``
hexo new '文章名'

``
文章分类需要添加标签
--category: 分类01




### 根目录结构
``
    参数	        描述	                                        
    source_dir	    资源文件夹，这个文件夹用来存放内容       
    public_dir	    公共文件夹，这个文件夹用于存放生成的站点文件
    tag_dir	        标签文件夹
    archive_dir	    归档文件夹
    category_dir	分类文件夹
    code_dir	    Include code 文件夹，source_dir 下的子目录
    i18n_dir	    国际化（i18n）文件夹
    skip_render	    跳过指定文件的渲染。匹配到的文件将会被不做改动地复制到 public 目录中。您可使用 glob 表达式来匹配路径。

### 新建文章
$ hexo new [layout] <title>
新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。

$ hexo new "post title with whitespace"
参数	描述
-p, --path	自定义新文章的路径
-r, --replace	如果存在同名文章，将其替换
-s, --slug	文章的 Slug，作为新文章的文件名和发布后的 URL
默认情况下，Hexo 会使用文章的标题来决定文章文件的路径。对于独立页面来说，Hexo 会创建一个以标题为名字的目录，并在目录中放置一个 index.md 文件。你可以使用 --path 参数来覆盖上述行为、自行决定文件的目录：

hexo new page --path about/me "About me"
以上命令会创建一个 source/about/me.md 文件，同时 Front Matter 中的 title 为 "About me"

注意！title 是必须指定的！如果你这么做并不能达到你的目的：

hexo new page --path about/me
此时 Hexo 会创建 source/_posts/about/me.md，同时 me.md 的 Front Matter 中的 title 为 "page"。这是因为在上述命令中，hexo-cli 将 page 视为指定文章的标题、并采用默认的 layout。