---
layout: post
title: 怎样在github编写自己的博客
---

有时候，你明明知道有官方的说明文档，却总是以为不如去百度，Google来的方便，不断的变换关键词去搜索，
而且还要去理解别人写的文档（没准他也不明白），其实你只要静下心来好好将官网文档看一遍，你会发现好多问题就解决了，
还是不懂，再搜索时也会一眼辨出是不是你要的内容，这样会更加省时，而且学得扎实。

有人就会说了，既然上面的内容都能在官网查到了，我还要你干嘛？没错，对于更权威的文档，我是很有必要推荐大家看的，而这里我仅仅是将本人搭建轻博客的过程做一个记录，
一来方便自己今后查看，二来给你做一个参考，而且前一个目的才是主要的，仅此而已。

markdown是我非常喜欢的一款用于排版的语法，本文主要是介绍的是利用markdown写自己的博客，利用gitHub托管自己的博客，利用jekyll作为解析引擎， 
对于想实现我说的这些步骤的读者，可能需要你有点计算机背景

### 必备条件

1. gitHub账号 [https://github.com/](https://github.com/)
2. 本地安装版本控制工具git [http://gitref.org/zh/](http://gitref.org/zh/)
4. 熟悉markdown语法 [http://www.appinn.com/markdown/](http://www.appinn.com/markdown/)
5. 知道jekyll解析引擎[http://jekyll.bootcss.com](http://jekyll.bootcss.com)
6. gitHub pages 了解 [https://pages.github.com/](https://pages.github.com/)

### Markdown

Markdown 的目标是实现「易读易写」. 
Markdown 的语法全由一些符号所组成，这些符号经过精挑细选，其作用一目了然。比如：在文字两旁加上星号，看起来就像*强调*。Markdown 的列表看起来，嗯，就是列表。Markdown 的区块引用看起来就真的像是引用一段文字，就像你曾在电子邮件中见过的那样。


### jekyll 

Jekyll 是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过 Markdown （或者 Textile） 以及 Liquid 转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。Jekyll 也可以运行在 GitHub Page 上，
也就是说，你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是完全免费的。


##### 中文文档：[http://jekyll.bootcss.com](http://jekyll.bootcss.com)

如果安装jekyll出现下面错误

    ERROR:  Could not find a valid gem 'bundler' (>= 0), here is why:
              Unable to download data from https://rubygems.org/ - SSL_connect retur
    ned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (
    https://rubygems.org/latest_specs.4.8.gz)
    Successfully installed bundler-1.7.12
    Parsing documentation for bundler-1.7.12
    WARNING:  Unable to pull data from 'https://rubygems.org/': SSL_connect returned
    =1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (htt
    ps://rubygems.org/latest_specs.4.8.gz)
    1 gem installed
    

参照这篇文章： [http://ruby.taobao.org/](http://ruby.taobao.org/)


### 添加评论功能

多说：多说是追求最佳用户体验的社会化评论框，为中小网站提供新浪微博、QQ（QQ空间和腾讯微博）、人人、开心网、豆瓣、网易微博、搜狐微博、百度、淘宝、Google等多帐号登录并评论功能。它帮你搭建更活跃，互动性更强的评论平台。具有众多实用特性，功能强大且永久免费
[http://dev.duoshuo.com/docs](http://dev.duoshuo.com/docs)

        <!-- 多说评论框 start -->
        <div class="ds-thread" data-thread-key="{{ page.id }}" data-title="{{ page.title }}"
             data-url="{{ site.baseurl }}/{{ page.url }}"></div>
        <!-- 多说评论框 end -->
        <!-- 多说公共JS代码 start (一个网页只需插入一次) -->
        <script type="text/javascript">
            var duoshuoQuery = {short_name: "wangjuntytl"};
            (function () {
                var ds = document.createElement('script');
                ds.type = 'text/javascript';
                ds.async = true;
                ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
                ds.charset = 'UTF-8';
                (document.getElementsByTagName('head')[0]
                || document.getElementsByTagName('body')[0]).appendChild(ds);
            })();
        </script>
        <!-- 多说公共JS代码 end -->

这段代码有4个地方要填

    data-thread-key填上 大括号大括号 page.id 大括号大括号
    data-title 填上 大括号大括号 page.title 大括号大括号
    data-url 填上 your web site/大括号大括号 page.url 大括号大括号
    short_name 注册多说账号的时候让你填的二级短域名




