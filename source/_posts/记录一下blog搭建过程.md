---
title: 记录一下blog搭建过程
date: 2020-08-14 15:33:40
tags: 
- BLOG
- HEXO
- 服务器
categories: 
- 学习
- BLOG
---
# 前言
年初的时候接了一个前端项目，刚好阿里云搞活动，白嫖了阿里云半年服务器，经过半年的学习打算正式搞个blog（之前那个博客随着白嫖服务器过期而失踪了），这次我又续费了3年的阿里云学生机，续费后第一个想到的就是重新搭建一个自己的博客，考虑到学生机只有1M的带宽，便搞个静态博客玩玩吧。

# 环境部署
* 使用环境
  * [x] 本地环境：windos10
  * [x] 服务器系统：centos
* 本地部署
  * [x] GIT
  * [x] NODEJS
  * [x] HEXO
* 服务器部署
  * [x] GIT
  * [x] NGINX  
<br>
使用git自动化部署博客

# git
* 安装(本地服务器大致相似)  
  `yum install git`

* test  
  `git --v`  

* 创建git用户（服务端）  
  `adduser git `

* 建立仓库  
`cd /home/git `# 进入 git 用户目录  
`mkdir blog && chown -R git:git $_`  # 博客文件夹,设置权限   
`mkdir projects && chown -R git:git $_ && cd $_`   # 创建项目目录，设置权限并进入  
`git init --bare hexo.git && chown -R git:git $_`  # 创建博客裸仓，设置权限`

* 通过ssh链接仓库（已将公钥添加至服务端）
  
# HEXO
* 修改配置 _config.yml （ hexo 配置文件）  
`deploy:`  
  `type: git`  
  `repo: git@ip:/home/git/projects/hexo.git`#ip 为服务器ip  
  `branch: master`

* 自动部署
进入git用户下的`projects/hexo.git/hooks`  
进入`hook`  
`mv post-update.sample post-update`# 重命名 post-update     
`vi post-update `# vim 进行编辑  
* 进入编辑模式
输入`git --work-tree=/home/git/blog --git-dir=/home/git/projects/hexo.git checkout -f `  
* 接着就是日常
  * `hexo clean`
  * `hexo g`
  * `hexo d`
  * `hexo s`
  * `……`

# nginx 反向代理服务器
>略  
不想写了
  
# 结束  
动起来，别光看。