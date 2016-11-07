---
title: createBlog
date: 2016-11-01 19:54:02
tags: [hexo, git, yilia, gitHub Pages]
---

# 利用hexo和github pages搭建自己的博客

## 何为Hexo?
> `hexo`的官方定义是:一个简单地,轻量的,基于node的一个静态博客网站框架;可以方便快捷地生成静态网页托管在github上;

## 何为gitHub Pages?
> 可以被认为是用户编写的,托管在github上的静态网页.空间免费哦!<i class="icon-price-tags"></i>

## 何为github?
> 这个问题小编<i class="icon-user"></i>就不回答了,偷偷告诉你github是一个程序员必不可少的"左膀右臂";有了它,你会觉得...还没有?那还在等什么?赶紧点击[注册](https://github.com)啊!

搭建过程需要配置很多文件,也需要很多知识(那种看看就会的,相信我,`绝对比做一个产品的需求简单多了`),下面就进入正题吧!

<!--more-->

# 环境准备

## 安装node

安装?点击去[官网](https://www.npmjs.com/)啊!

## 安装git

yes,点击去[官网](https://git-scm.com/book/en/v2);

## 注册github(不多说了,作为程序员没有github账号的话,允许我鄙视5秒钟)

## 安装Hexo

> npm install -g hexo

然后进行初始化我们的博客项目:

> hexo init <folder>

这样就生成了我们博客项目的文件夹,然后到该文件夹下看一下生成的文件内容,在该文件夹下执行下面代码,安装依赖:

> npm install

在该文件夹下你会发现下面这些文件:
1. _config.yml : 博客项目的配置文件
2. package.json : 项目源信息以及项目依赖信息
3. scaffolds : 脚手架文件夹，当我们创建一个新的文章时，hexo就是根据scaffolds内的文件工作的
4. source : 博客项目源文件夹，里面的文件将被渲染处理并放入public文件夹
5. themes : 主题文件夹(我们可以通过更改该文件下的文件内容,更改我们博客的主题风格)

接下来我们就可以写新的文章了,通过下面的命令:

> hexo new 'filename'

执行完后我们会看到在`source/_posts`文件夹下会产生一个filename.md文件,该文件是按照markdown规格来写的,用来生成我们新博客的内容,所以如果不会就点击[学习MarkDown](https://www.zybuluo.com/mdeditor)
写完文章后需要执行下面的命令进行编译:

> hexo generate

And then, execute next:

> hexo server

就在本地4000端口起了服务了;还等什么?localhost:4000看效果啊!(<i class="icon-hour-glass"></i>小编忘记吃药了,见谅)
很神奇的发现带有默认主题的博客就出现了,为什么会有样式?为什么?为什么?不要激动,还记得那个`themes`文件夹么?对就是它,不要怕点进去,看一下代码,尤其是`source-src`文件夹下的文件,很亲切吧,想改成自己喜欢的?just do it!
如果想切换主题风格,点击[hexo主题库](https://github.com/hexojs/hexo/wiki/Themes)自由地切换吧,安装新的主题到`themes`文件夹下,然后要对该主题下的`_config.yml`文件进行配置<i class="icon-alarm"></i>(不是
项目根目录下的`_config.yml`,它也需要配置,但有别的用处)

## 部署到github(默认你已经有账号)

在自己的账户下创建一个仓库，命名为 `username.github.io` ，注意：这个仓库名称是固定的，将`username`替换成你的github用户名。然后修改博客根目录下的配置文件`_config.ylm`:

```
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: master
```

配置好之后，还需要在自己的github上添加 SSH Key，如何生成SSH Key以及如何配置添加,请点击[SSH Key](https://help.github.com/articles/generating-an-ssh-key);
添加SSH Key后安装一个插件:

>npm install --save hexo-deploy-git

然后,last step:

>hexo deploy

部署成功,赶紧浏览器输入username.github.io查看自己的博客吧!项目根目录下的`_config.yml`配置详情,点击[_config.yml](https://hexo.io/docs/configuration.html)

当然也可以点击[博主](https://github.com/barefootChild/blog_file)查看我的博客配置文件,感觉可以就点个star吧!

# 通篇博文看下来,或许你会感觉博主很懒,什么配置啊,安装啊都连接到其它处!

 没错,具体配置什么的,要自己去官网看,官方文档写得很仔细,也是最权威的地方,每个人看都希望追求个性,还望大家,自己查阅,按照自己的喜好配置,博主只是提供路径!



