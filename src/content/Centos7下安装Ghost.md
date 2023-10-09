---
layout: post
title: Centos7下安装Ghost
image: img/testimg-cover.jpg
author: [Ghost]
date: 2023-09-29T07:03:47.149Z
tags:
  - Tests
  - 网络
---

ghost是基于node.js的新一代博客平台——相比日渐臃肿、无所不能的WordPress，Ghost更专注于博客，在官网上打出“只做博客平台”的口号，因此相对WordPress会更加简洁易用。<!-- more -->

# 1.基础配置
## 1.1安装node.js及nginx
```curl --silent --location https://rpm.nodesource.com/setup_6.x | sudo bash -
sudo yum -y install nodejs
sudo yum install gcc-c++ make
yum install -y nginx
```

# 2.安装ghost-cli
ghost-cli是1.0版本之后出现的新工具。
安装ghost-cli:
`sudo npm i -g ghost-cli`

# 3.ghost安装
以文件夹blog为例，我们可以先创建一个叫做blog的文件夹安放我们的博客文件:
`mkdir blog`
`cd blog`

然后就可以使用ghost-li来安装ghost了
`ghost install [version]`
version可为1.0.2等等,如果要安装最新版只需ghost install即可.

warning:
此上安装方法数据库默认为mysql,如果使用sqlite3可以使用一下语句:
`ghost install --db sqlite3 --dbpath ./content/data/ghost.db`

接下来会有几行warning:
System checks failed with message: 'Linux version is not Ubuntu 16'
Some features of ghost-cli may not work without additional configuration
...

我选择了无视它继续往前走
后面就会是让你输入blogUrl
是否配置nginx SSL Systemd
按照自己意愿做出选择就好.

# 4.使用
在nginx.conf做好配置后就能够重启nginx了，如果域名解析正确就能够使用了

进入 http://你的域名/ghost
该网址为ghost的管理后台

# 5.ssl
出于安全策略， Let’s Encrypt 签发的证书有效期只有 90 天，所以需要每隔三个月就要更新一次安全证书，3个月到期后要重新生成SSL进入服务器输入：
`certbot renew`

