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

注册[Disqus](http://disqus.com/)账号。选择Universal Code，将代码复制到模版中。复制到文章框架中即可。

    <div id="disqus_thread"></div>
         <script type="text/javascript">
        /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
        var disqus_shortname = 'wangxt'; // required: replace example with your forum shortname

        /* * * DON'T EDIT BELOW THIS LINE * * */
        (function() {
            var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
            dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
            (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
        })();
        </script>
    
    <noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
    <a href="http://disqus.com" class="dsq-brlink">comments powered by <span class="logo-disqus">Disqus</span></a>

##使用github pages建立博客

create new repository，项目名称为github账户名.github.io。创建好之后点击settings进入项目管理页配置。之后提交一个index.html文件，push到GitHub的master分支。第一次页面生效需要一些时间，大概10分钟左右。即创建了一个page，参见
[GitHub Pages](https://pages.github.com/)官方文档。网站可以使用前面所说的Jekyll进行托管。

##使用git进行版本管理

###安装与配置

下载windows版本的[git](http://git-scm.com/downloads)。

打开Git Bash设置并与github连接。

* 检查SSH Keys的设置

    $ cd ~/.ssh

* 如果存在key文件，需要备份和溢出原来的key

    $ ls
    config  id_rsa  id_rsa.pub  known_hosts
    $ mkdir key_backup
    $ cp id_rsa* key_backup
    $ rm id_rsa*

* 生成新的SSH key

    $ ssh-keygen -t rsa -C "邮件地址"

然后按照要求输入两次密码即可成功设置。

* 添加key到github

用文本编辑工具打开id_rsa.pub文件，复制全部内容。然后在GitHub主页上点击Account Settings按钮，选择SSH Keys选项，把复制的内容粘贴进去，点击Add key即可。

* 测试是否连接

输入以下代码，然后输入yes。

    $ ssh -T git@github.com

* 设置账号信息以便以后提交时使用

    $ git config --global user.name "name"
    $ git config --global user.email "your_email"
    
##常用的git命令

更新github中的文件

    git clone 地址
    git add .     //增加全部文件
    git status     //检查git状态
    git commit -am "message"   //提交
    git push origin master   //将本地master分支放到远端


删除github某repository上的内容

    cd [repository/files]
    git rm ...

同步远端内容到本地

    git pull

##参考文献

[搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)  
[Markdown语法](http://zh.wikipedia.org/wiki/Markdown#.E5.88.97.E8.A1.A8)  
[jekyll](http://jekyllcn.com)  
[Github Page Basics](https://help.github.com/categories/20/articles)  
[使用Github Pages建独立博客](http://beiyuu.com/github-pages/)  
[Jekyll安装配置](http://www.soimort.org/posts/101/)
