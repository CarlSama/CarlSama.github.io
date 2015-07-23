---
layout : post
time : 2015-4-16
title : 在去往GitHub & Jekyll的路上
category : tutorial
keywrods : GitHub, Jekyll
tags : GitHub
description : 我在使用GitHub和Jekyll搭建个人博客时遇到的问题
---

## 前言

刚刚几分钟之前,终于成功完整的搭建了自己的第一个个人博客.其实昨天下午就已经差不多搭建完成,可以一致卡在了一个问题上,到时我花费了很多的时间来测试,修正.

现在我把自己在配置github + jekylly过程中遇到的问题写下来,希望对他人有所帮助.
</br></br>

## 环境准备

关于github + jekyll的软件需求,[sosohu](http://sosohu.github.io/experience/2015/02/06/Github-Jekyll-Markdown开启写作新时代.html)这篇文章已经有所介绍.在此不加赘述.

不过给大家一点建议就是,最好能够自己能够到[ruby](www.ruby-lang.org/en/)的官网去下载源码包,本地编译安装.
</br></br>

## 关于Disqus插件

在[sosohu](http://sosohu.github.io/experience/2015/02/06/Github-Jekyll-Markdown开启写作新时代.html)的blog里面对disqus的本地化配置也有所介绍.

在这里要提醒大家的一点时,当在[Disqus](https://disqus.com)中注册了账号以后,一定要记得去profile > Edit Profile > Moderation中添加自己的moderator,不然系统会报出"We were unable to load Disqus.."的错误.

此外,在修改.yml文件时,一定要注意的是每个key与后面的':',以及再之后的value之间都应该有空格间隔.例如:

{% highlight C++ %}
	key : value
{% endhighlight %}
</br></br>

## 关于cnzz插件

强烈建议大家去[cnzz](http://new.cnzz.com/v1/login.php?siteid=1000067280)注册自己的数据分析账号,并将所有配置文件中相应的字段替换. 
</br></br>

## 文件目录结构分析

通过许多测试,初步确定了各个目录的大致功能,分析如下:

| File Name | Function |
| :-------: | :------: |
|404.html     |404 Not found|
|about.md     |博客页面上栏"About"的生成模板|
|archive.md   |博客页面上栏"Archive"的生成模板|
|categories.md|博客页面上栏"Category"的生成模板|
|config.yml   |用户个性化配置文件|
|includes/    |博客的主题模板|
|index.md     |博客主页索引生成模板|
|layouts/     |博客文章页的生成模板|
|plugins/     |插件|
|posts/       |用户存放博客的地方|
|site/        |用户运行jekyll build .后将_posts/下的MarkDown转换为的Html文件|

</br>

## 一点建议

在sosohu的介绍下,我强烈的建议你能够使用两个git branch: 

1.	master分支做最终的提交使用; 

2.	source分支做代码编辑与编译.

</br>

## 最后

感谢[sosohu](http://sosohu.github.io/experience/2015/02/06/Github-Jekyll-Markdown开启写作新时代.html)的blog对我的搭建工作提供的帮助.

快来动手搭建自己的blog吧 :)   Have fun
</br></br>

![img](http://carlsama.github.io/assets/image/catoon/death.jpg)
