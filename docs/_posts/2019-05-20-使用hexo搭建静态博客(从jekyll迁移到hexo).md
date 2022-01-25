---
layout: post
title: "使用hexo+github pages 搭建静态博客(从jekyll迁移到hexo)"
date: 2019-05-20 10:10:03
categories:
 - Skills
tags:
 - Hexo
---

笔者以前使用的 `jekyll+github pages` 的方式搭建博客, 后来从jekyll 迁移到了 hexo, 并不是说 jekyll 比 hexo 差.技术没有好坏, 只有合不合适, 我完全是自己折腾着玩儿. 在迁移的过程中也遇到一些问题, 下面就简要说明一下 hexo + github pages 的搭建步骤. 认真阅读原文, 你将能独立搭建一个静态博客.



我们分为两个步骤讲解然后讲解一下如何使用主题(next):

一, 使用hexo建站

二, 部署到 github pages

三, next 主题的使用

### 使用hexo建站

根据[官方文档](https://hexo.io/zh-cn/docs/index.html)你可以很轻松的搭建一个静态博客网站.

几个hexo常用的命令:

	hexo new "postName" #新建文章
	hexo new page "pageName" #新建页面
	hexo generate #生成静态页面至public目录
	hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
	hexo deploy #将.deploy目录部署到GitHub
	hexo help  # 查看帮助
	hexo version  #查看Hexo的版本
	
命令简写如下:

	hexo n == hexo new
	hexo g == hexo generate
	hexo s == hexo server
	hexo d == hexo deploy
	hexo g -d == hexo g + hexo d

### 部署到 github pages

部署到 github pages 也非常简单(后面会讲到), 再部署之前我们还有很多准备工作要做.

#### 申请[github](https://github.com/join?source=header-repo)账号.

申请步骤按照提示完成即可

#### 新建仓库

![](/assets/images/ssh0.png)
仓库名称有着严格的规范,如下:

`自己的github账号名称.github.io`

例如,我的github账号为 zhangsan, 那么仓库的命名<mark>一定是</mark>: *zhangsan.github.io*

#### 配置SSH
首先我们普及一下 SSH和https的区别:

>1. SSH可以随意克隆github上的项目，而不管是谁的；而后者则是你必须是你要克隆的项目的拥有者或管理员，且需要先添加 SSH key ，否则无法克隆。
2. https url 在push的时候是需要验证用户名和密码的；而 SSH 在push的时候，是不需要输入用户名的，如果配置SSH key的时候设置了密码，则需要输入密码的，否则直接是不需要输入密码的。

##### 配置的SSH的方式

如下图:

![](/assets/images/ssh1.png)

在左侧功能区选择` SSH and GPG keys `, 在右侧 点击 `add new SSH` 按钮.会出现如下图

![](/assets/images/ssh2.png)

title: ssh的名称, 可以随便写.
key: 公钥. 

如何生成公钥呢? 步骤如下:

1、首先需要检查你电脑是否已经有 SSH key 

运行 git Bash 客户端，输入如下代码：
	
	$ cd ~/.ssh
	$ ls

这两个命令就是检查是否已经存在 `id_rsa.pub `或 `id_dsa.pub `文件，如果文件已经存在，那么你可以跳过步骤2，直接进入步骤3。

2、创建一个 SSH key 

	$ ssh-keygen -t rsa -C "your_email@example.com"

代码参数含义：

	-t 指定密钥类型，默认是 rsa ，可以省略。
	-C 设置注释文字，比如邮箱。
	-f 指定密钥文件存储文件名。
	
3、添加你的 SSH key 到 github上面去

首先你需要拷贝 id_rsa.pub 文件的内容，你可以用编辑器打开文件复制，也可以使用终端打开, 这里就是用终端打开. 

 	cat id_rsa.pub
 	
终端会显示该公钥的密文, 赋值之后粘贴到上图中的key中即可.

4、测试一下该 SSH key

在git Bash 中输入以下代码

	$ ssh -T git@github.com
	
如果出现如下信息则证明操作成功:

	Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
	
至此第二步中添加 SSH 完成.下面开始将第一步建的站部署到github pages 上. 只需一步即可完工:

<mark>切换到hexo博客根目录, 运行` hexo g -d `即可</mark>

### 如何使用 [next主题](https://github.com/theme-next/hexo-theme-next)

终端切换到hexo博客根目录,然后执行如下命令:


	$ git clone https://github.com/theme-next/hexo-theme-next themes/next

然后在站点配置文件中做如下修改:

	theme: next

然后重启 hexo 服务(先停止: ctrl + c, 然后运行 hexo s), 重新生成静态页面(hexo g)

hexo 的 next主题的配置, 在n[ext官网](http://theme-next.iissnan.com)上介绍的十分详细.

### 搭建博客过程中遇到的问题
一. 博客中的图片无法显示.

解决方式: 
将图片资源全部存放在 source 目录下的 assets(自己建的文件夹)文件夹中. 在博客中使用md语法引用图片 `![图片描述](/assets/imageName.png)` 如果有下级目录, 按照目录结构写即可.

二. 按照[next官网](http://theme-next.iissnan.com)教程配置local_search无法搜索.

原因:发布的博文中有特殊字符(非utf-8字符集),主要是由于在直接复制网上资料时错误的将特殊字符复制进来了, 你可以将博文先全部删除, 只留一篇测试博文, 然后重新发布一下,试试搜索功能是否正常. 或者直接访问 `http://localhost:4000/search.xml`, 然后打开元素检查来查看具体的错误.(错误定位有可能不准确).

解决方法:找到特殊字符集, 然后删除

三. 如何在文章列表中显示摘要.

在 next主题中配置显示摘要, 配置如下, 但是不起作用.

	auto_excerpt:
	  enable: true
	  length: 150
	  
解决方法: 使用`<!--more-->`来隔断摘要和正文, 在博文中` <!--more--> `之前的会被自动显示成摘要, 之后的要在点击`read more`之后才会显示

四. 写完博客之后, 发布但是不显示最新发布的博客

经过分析, 错误原因是博客开始的写法不对, 错误写法如下:

	---
	layout:post
	title:"使用hexo+github pages 搭建静态博客(从jekyll迁移到hexo)"
	date:2019-05-20 10:10:03
	categories:
	 - Skills
	tags:
	 - Hexo
	---
以上写法是错误的, 在`layout:post` 冒号后要加空格, 正确的写法如下:

	layout: post
	title: "使用hexo+github pages 搭建静态博客(从jekyll迁移到hexo)"
	date: 2019-05-20 10:10:03
	categories:
	 - Skills
	tags:
	 - Hexo
	---
