---
title: 虚拟环境Ubuntu和Windows
tags:
  - python
  - ubuntu
  - windows
categories:
  - 专业
  - python
keywords: 自己关于python的学习知识
abbrlink: 104615db
date: 2019-08-02 14:46:32
---
# 引言

关于自己学习python的笔记，仅供参考。

<!--more-->



## Linux下安装虚拟环境
#### 安装
    sudo apt-get install python-virtualenv
    
#### 使用方法
    virtualenv [虚拟环境名称] 

> 如，创建**ENV**的虚拟环境

    virtualenv ENV
    # which python* # 查看python安装位置
    mkvirtualenv  -p /usr/bin/python3 python3
    deactivate
    mkvirtualenv  -p /usr/bin/python2 python2
    deactivate
    
> 默认情况下，虚拟环境会依赖系统环境中的site packages，就是说系统中已经安装好的第三方package也会安装在虚拟环境中，如果不想依赖这些package，那么可以加上参数 --no-site-packages建立虚拟环境

    `virtualenv --no-site-packages [虚拟环境名称]`
> 启动虚拟环境
    ```
    cd ENV
    source ./bin/activate
	```
> 注意此时命令行会多一个(ENV)，ENV为虚拟环境名称，接下来所有模块都只会安装到该目录中去。



> 退出虚拟环境
    
    `deactivate`

### Virtualenvwrapper
Virtaulenvwrapper是virtualenv的扩展包，用于更方便管理虚拟环境，它可以做：

1. 将所有虚拟环境整合在一个目录下
2. 管理（新增，删除，复制）虚拟环境
3. 切换虚拟环境

#### 安装
    `sudo apt-get install virtualenvwrapper`
```
    # 查找安装位置

    whereis virtualenvwrapper.sh
```
> 创建目录用来存放虚拟环境

    `mkdir $HOME/.virtualenvs`

> 在~/.bashrc中添加行：

    `export WORKON_HOME=$HOME/.virtualenvs`

> 在~/.bashrc中添加行：
```
    # whereis virtualenvwrapper.sh
    source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
```

> 运行：

    `source ~/.bashrc`

### 在虚拟环境安装Python套件
Virtualenv 附带有pip安装工具，因此需要安装的套件可以直接运行：

    `pip install [套件名称]`

如果没有启动虚拟环境，系统也安装了pip工具，那么套件将被安装在系统环境中，为了避免发生此事，可以在~/.bashrc文件中加上：

    `export PIP_REQUIRE_VIRTUALENV=true`

或者让在执行pip的时候让系统自动开启虚拟环境：

    `export PIP_RESPECT_VIRTUALENV=true`
    
    
## Windows

建立两个纯净的虚拟环境
```
    mkvirtualenv -p python3 py37
    mkvirtualenv -p python2 py27
```    
- workon Env  进入虚拟环境 
- pip list    查看已安装包列表 
- deactivate  退出虚拟环境




