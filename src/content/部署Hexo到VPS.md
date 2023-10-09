---
layout: post
title: 部署Hexo到VPS
image: https://cdn.jsdelivr.net/gh/10kilo/blogpic/photo/202310091532680.png
author: [Ghost]
date: 2023-09-30T07:03:47.149Z
tags:
  - Tests
  - 网络
---

什么是 Hexo？
 Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。本教程是通过本地生成静态文件，再通过`rsync`部署到 VPS 上，本教程采用 `Nginx` 服务器<!-- more -->

## 安装本机Hexo 的环境，参考 [Hexo官方文档](https://link.jianshu.com/?t=https://hexo.io/zh-cn/docs)


## 配置本机hexo

在 Hexo 目录下安装 `hexo-deployer-rsync`模块

`$ npm install hexo-deployer-rsync --save`

为_config.yml配置以下：<!-- more -->


```css
deploy:
  type: rsync
  host: VPS的IP地址或者域名
  user: root
  root: /var/www/hexo/  注：VPS存放网页的具体目录
  port: VPS的ssh端口号
  delete: false
```

## 配置本机ssh

安装 `Homebrew` :

`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" `

安装 ssh-copy-id:

`brew install ssh-copy-id`
ssh-copy-id命令可以把本地的ssh公钥文件安装到远程主机对应的账户下<!-- more -->

执行如下命令

`$ ssh-copy-id user@host`
其中将user替换为自己服务器用户名，host替换为对应的ip地址。通过此命令可以将本地的ssh公钥发送到目标主机上，然后登陆主机账户即可免密码登陆。

通过以下命令校验ssh免密登陆配置成功：

`ssh user@host echo "test"`

控制台输出test即表名配置成功

## VPS 上安装 Nginx 服务

SSH 连接 VPS 后，添加 CenOS 7 的 epel 软件包

`$ yum install epel-release`
安装Nginx

`$ yum install nginx`
启动 Nginx

`$ systemctl start nginx.service`
使用 firewalld 给防火墙添加规则允许 HTTP 以及 HTTPS (不知道 firewalld 的，请查看上篇文章)


```$ firewall-cmd --permanent --zone=public --add-service=http
$ firewall-cmd --permanent --zone=public --add-service=https
$ firewall-cmd --reload
```
设置 Nginx 自动跟随系统启动

`$ systemctl enable nginx.service`

## vps上配置 Nginx 

`$ vi /etc/nginx/conf.d/default.conf`
将以下内容替换原内容


```php
server {
  root /var/www/hexo; # 这里是你网站的路径 
  index index.html index.htm;

  server_name www.10kilo.cn 10kilo.cn *.10kilo.cn; 

  location / {
    try_files $uri $uri/ /index.html;
  }
}
```
重启一下 Nginx 服务

`$ systemctl start nginx.service`





