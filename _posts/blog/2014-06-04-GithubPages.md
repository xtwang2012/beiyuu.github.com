---
layout: post
title: 使用github pages建立独立的网站
description: 使用github pages可以方便并且免费的的建立自己的个人博客
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







