---
title: Hexo环境搭建
tags:
  - git
  - hexo
  - ssh
categories:
  - 专业
  - 前端
  - hexo
keywords:
  - hexo
  - git
  - ssh
abbrlink: 7d922bf5
date: 2019-07-28 09:21:12
---

## 前言

这篇文章介绍用GitHubPages和Hexo来搭建个人博客的环境搭建安装记录。

<!--more-->
## 准备环境

准备 node 和 git 环境。因为 Hexo 是基于 Node.js 驱动的一款博客框架



**下载安装Node.js**

Node.js官方下载地址：https://nodejs.org/

下载好之后，双击安装，一路next即可。

**下载安装git**

git官方下载地址：http://git-scm.com/download/
> 安装参考
> 掘金作者：[小杰哥001](https://juejin.im/user/5b6ba25a6fb9a04fca3ca37c)文章 --> 
> [git Windows版本安装教程](https://juejin.im/post/5c922e226fb9a070ca10351b)


**下载安装cmder(推荐)**
> 参考：知乎作者：林玮
> [Windows上的程序员神器——Cmder](https://zhuanlan.zhihu.com/p/28400466)

补充:

加入环境变量

`cd cmder.exe`所在的文件目录 添加右键命令:(`Cmder.exe /REGISTER ALL`)

在命令行中输入相应命令验证是否成功，如果成功会有相应的版本号

```
git version
node -v
npm -v
```
## 安装Hexo

右击任意位置，选择`Git Bash Here`, 或者选择`cmder here`。执行命令：

```
$ npm install -g hexo-cli
```
如果失败换国内镜像源试试。

`npm config set registry="http://registry.cnpmjs.org"` ，

然后再次执行`npm install -g hexo-cli`

### 创建hexo文件夹

在任意文件夹下（如E:\hexo），打开Git Bash，执行命令：

`hexo init`

Hexo 即会在目标文件夹建立网站所需要的所有文件。

安装依赖包

`npm install`

命令缩写
```
hexo generate = hexo g
hexo server = hexo s
hexo deploy = hexo d
hexo new = hexo n
```
新建完成后，指定文件夹的目录如下：

```
.
├── _config.yml # 网站的配置信息，您可以在此配置大部分的参数。 
├── package.json
├── scaffolds # 模版文件夹
├── source  # 资源文件夹，除 _posts 文件，其他以下划线_开头的文件或者文件夹不会被编译打包到public文件夹
|   ├── _drafts # 草稿文件
|   └── _posts # 文章Markdowm文件 
└── themes  # 主题文件夹
```

本地查看

执行以下命令：
```
hexo g
hexo s
```

然后到浏览器输入http://localhost:4000查看效果。

至此，本地博客已经搭建起来了，别人看不到的。

## 部署到GitHub

### 注册

GitHub官网：http://www.github.com

创建repository

creat new repository，Repository

name和自己的用户名相同。比如我的用户名为yangyang188，那么`Repository

name`就填
yangyang188.github.io。

> 注册：参考
> 作者：鱼先生，来源：segmentfault，原文：https://segmentfault.com/a/1190000017986794


### GitHub之SSH key

#### 1、设置Git的user.name和user.email

在第一次使用Git时，你需要告诉你的协同开发者，你是谁以及你的邮箱，在你提交的时候，Git需要这两个信息。具体通过以下命令设置：
```
git config --global user.name "yangyang188"

git config --global user.email "yangyang188@qq.com"
```
需要注意的是，这里的name随意，邮箱是你的联系邮箱，与github完全无关。
查看配置命令：

`git config --list`

#### 2、生成SSH密钥

`ssh-keygen -t rsa -C "voidking@qq.com"`，按3个回车，密码为空。

```
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa):
Created directory '/c/Users/Administrator/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/Administrator/.ssh/id_rsa.
Your public key has been saved in /c/Users/Administrator/.ssh/id_rsa.pub.
The key fingerprint is:
e6:9e:e7:ae:ad:ee:9f:f0:e5:6b:60:63:85:e8:cd:ae yangyang@qq.com

```
在`C:\Users\Administrator\.ssh`下，得到两个文件`id_rsa`和`id_rsa.pub`。

需要注意的是，命令中的-C参数，后面跟的内容是注释。也就是说，内容随意，与github完全无关。

#### 3、在GitHub上添加SSH密钥

打开`id_rsa.pub`，复制全文。https://github.com/settings/ssh ，`Add SSH key`，粘贴进去。

#### 4、测试


`ssh git@github.com`，提示：
```
The authenticity of host 'github.com (192.30.252.128)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.252.128' (RSA) to the list of known hosts.
Hi voidking! You've successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.
```
> 参考：生成SSH密钥，作者：VoidKing
原文：https://www.voidking.com/dev-hexo-build-environment/



_config.yml

编辑E:\hexo下的_config.yml，修改Deployment部分：

```
# deploy:
#   type: git
#   message: [message]
#   repo:
#     github: <repository url>,[branch]
```
我的配置：

```
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/yangyang1188/yangyang188.github.io.git
  branch: master
```

### 部署

`hexo d`，执行该命令，报错：

```
ERROR Deployer not found: git
```
执行命令：

`npm install hexo-deployer-git --save`，再次执行`hexo d`


### 访问测试

访问：http://yangyang.github.io/ ，可以看到，Hexo已经搭建成功！

## 参考文档



[官方文档](http://hexo.io/docs/)

[Hexo环境搭建](https://www.voidking.com/dev-hexo-build-environment/)

[git Windows版本安装教程](https://juejin.im/post/5c922e226fb9a070ca10351b)

[Windows上的程序员神器——Cmder](https://zhuanlan.zhihu.com/p/28400466)

[超详细Hexo+Github Page搭建技术博客教程【持续更新】](https://segmentfault.com/a/1190000017986794)







