---
title: 记hexo使用过程
date: 2020-08-22 12:11:43
tags:
- 学习
- HEXO
- BLOG
categories:
- 学习 
- BLOG
---
# 1.博客的架构  
**先搞明白Hexo博客从搭建到自动发布的架构，才能更好的理解我们每一步进行的操作。**  

整个流程就是本地将 *.md 渲染成静态文件，然后Git推送到服务器的repository,服务器再通过 git-hooks 同步网站根目录。  

# 2.搭建流程  

+ 第一步： 安装node.js以及本地Hexo初始化
+ 第二步： 服务器环境搭建，包括安装 Git 、Nginx配置 、 创建 git 用户
+ 第三步： 使用Git自动化部署发布博客  

# 3.服务器环境搭建
## 3.1.安装Git和NodeJS (CentOS 环境)
```
yum install git
#安装NodeJS
curl --silent --location https://rpm.nodesource.com/setup_5.x | bash -
```
## 3.2. 创建git用户
```
adduser git
chmod 740 /etc/sudoers
vim /etc/sudoers
```
……  
☝以上届时再自行学习  
## 3.3.安装nginx  
+ 因为我们是拿nginx做Web服务器，所以我们需要安装部署好nginx。
+ 我们可以专门为hexo创建一个部署目录，比如我创建了/home/www/hexo文件夹，并把nginx的配置文件nginx.conf中的部署目录改为/home/www/hexo，配置文件一般在/usr/local/nginx/conf里；同样可以使用默认目录，nginx的默认目录为/var/www/html，如果使用宝塔的主机面板，则默认的部署目录为/www/wwwroot/hexo

# 4.本地Hexo程序
## 4.1.初始化Hexo博客
**查看Hexo官方文档**

# 5.自动化部署
## 5.1.服务器上建立git裸库
创建一个裸仓库，裸仓库就是只保存git信息的Repository, 首先切换到git用户确保git用户拥有仓库所有权
一定要加 --bare，这样才是一个裸库。
```
su git
cd ~
git init --bare blog.git
```
## 5.2.使用 git-hooks 同步网站根目录
在这里我们使用的是 post-receive这个钩子，当git有收发的时候就会调用这个钩子。 在 ~/blog.git 裸库的 hooks文件夹中，新建post-receive文件。
```
vim ~/blog.git/hooks/post-receive

#!/bin/sh
git --work-tree=/path/to/www --git-dir=~/blog.git checkout -f
```
保存后，要赋予这个文件可执行权限
```
chmod +x post-receive
```
## 5.3.配置_config.yml,完成自动化部署
然后打开 _config.yml, 找到 deploy  
```
deploy:
    type: git
    repo: git@SERVER:/home/git/blog.git    //<repository url>
    branch: master            //这里填写分支   [branch]
    message: 提交的信息         //自定义提交信息 (默认为 Site updated: {{ now('YYYY-MM-DD HH:mm:ss') }})
```
保存后，尝试将我们刚才写的"hello hexo"部署到服务器
```
hexo clean
hexo generate --deploy
```
访问服务器地址，就可以看到我们写的文章"Hello hexo",以后写文章只需要：
```
hexo new "Blog article name"
···写文章
hexo clean && hexo generate --deploy
```