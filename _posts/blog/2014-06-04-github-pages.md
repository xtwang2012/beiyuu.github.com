---
layout: post
title: 使用github pages建立独立的网站
description: 使用github pages可以方便的建立自己的个人博客，而且有300M免费空间
category: blog
---

##Jekyll

Jekyll是简洁的，特别是针对博客平台的静态网站生成器，它使用目录作为网站布局的基本框架，并可以在上面运行标记语言（Textile、Markdown或者Liquid），最终生成一个完整的静态web站点。

基本的Jekyll结构如下：

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




##参考文献
[搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)
