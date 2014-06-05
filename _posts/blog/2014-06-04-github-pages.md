---
layout: post
title: 使用github pages建立独立的网站
description: 使用github pages可以方便的建立自己的个人博客，而且有300M免费空间
category: blog
---

##Jekyll

Jekyll是简洁的，特别是针对博客平台的静态网站生成器，它使用目录作为网站布局的基本框架，并可以在上面运行标记语言（Textile、Markdown或者Liquid），最终生成一个完整的静态web站点。

###基本的Jekyll结构

    |-- _config.yml
    |-- _includes
    |-- _layouts
    |   |-- default.html
    |   `-- post.html
    |-- _posts
    |   |-- 2007-10-29-why-every-programmer-should-play-nethack.textile
    |   `-- 2009-04-26-barcamp-boston-4-roundup.textile
    |-- _site
    `-- index.html

每部分功能的详述：

####_config.yml

Jekyll的配置文件。Jekyll可以在文件中配置，也可以作为命令行的标记类[配置](http://jekyllcn.com/docs/configuration/)。

####_includes

可以用来存放一些可复用的模块，这些模块可以使用Liquid标签调用。例如：可以用这个标签  {% include file.ext %} 来把文件_includes/file.ext 包含进来。

####_layouts

用来存放文章模版，可以使用[YAML](http://jekyllcn.com/docs/frontmatter/)进行选择。

####_posts

存放文章，文章的命名必须符合YEAR-MONTH-DAY-title.MARKUP，文章的链接显示方式可以通过在文章中YAML部分指定permalinks来实现。

####_site

存放Jekyll完成转换后最终生成的文档。不必关心。可以讲这个目录放进.gitignore文件中。

##添加Disqus管理评论

注册[Disqus](http://disqus.com/)账号。选择Universal Code，将代码复制到模版中。



##参考文献

[搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)

[Markdown语法](http://zh.wikipedia.org/wiki/Markdown#.E5.88.97.E8.A1.A8)

[jekyll](http://jekyllcn.com)
