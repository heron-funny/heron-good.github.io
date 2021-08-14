---
layout: post
title:  "linux查找命令-find"
date:   2021-06-08
categories:
 - Turorial
tags:
 - Skills
---

### Nginx在 MacOS中的安装,配置以及使用

### 安装

1. 从[nginx官网](http://nginx.org/en/download.html)现在安装包

2. 下载完成之后切换到nginx目录, 然后以管理员身份执行 `sudo ./configure`,可能会报错, 根据错误信息执行或安装对应的模块即可, 常见错误如下:

   1. `./configure: error: the HTTP rewrite module requires the PCRE library.`解决办法也很简单:
      - 首先到[pcre官网](https://ftp.pcre.org/pub/pcre/)下载安装包,下载`pcre-8.45`版本
      - 使用执行` ./configure;make;make install`

3. 安装nginx

   1. 先切换到nginx的包目录
   2. 执行 `./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.35`, 其中--with-`pcre=/usr/local/src/pcre-8.35`是依赖库pcre的安装包包路径,如果已经安按照第一步安装了,可以不使用此参数

4. 安装成功之后nginx的可执行文件在如下目录: `/usr/local/nginx/sbin`,可以切换到此目录之后,执行 `./ningx -v`查看nginx的版本号

5. macOS环境变量的配置

   1. 如果使用的zsh, 这样配置, 打开.zshrc, 添加如下两行:

      ```
      export NGINX_HOME="/usr/local/nginx"
      export PATH=$PATH:$NGINX_HOME/sbin
      ```

   2. 如果使用bash,这样配置, 打开 .bash_profile, 也添加如下两行:

      ```
      export NGINX_HOME="/usr/local/nginx"
      export PATH=$PATH:$NGINX_HOME/sbin
      ```

      然后执行`source ~/.bash_profile`,也可以重启终端生效.



参考资料: 

1. [nginx安装以及基本命令](https://www.kuangstudy.com/bbs/1398876782639255554)

