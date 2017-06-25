---
layout: post
title:  "使用jenkins构建前端CI"
date:   2017-06-25
excerpt: ""
tag:
- jenkins
comments: true
---

## 何谓CI
>持续集成（CI）指的是，频繁地（一天多次）将代码集成到主干。

## 为何要构建CI
    这里介绍一下使用CI的项目背景：
    
    公司前端项目在开发过程中使用本地服务器调试，但是在提测阶段交付给QA时需要保持测试环境与线上环境一致（防止线上环境一些莫名其妙的问题产生）。所以项目需要部署至测试服务器交付给QA测试。  

    实际交付的过程是：
    本地代码更新 -> 本地打包 -> tag上传至测试服务器 -> QA测试

    上面是一条正常的交付流程，但是在实际情况中，QA测试之后提出的问题开发人员在修改之后不得不中断手中的工作去进行项目的打包部署，导致开发人员的工作效率下降。

    然后公司大佬就给出建议使用jenkins来搭建前端的CI。

## 我这里构建前端CI的思路
    
    push至CI监听的git分支 -> CI polling -> git clone -> build tag -> tag传输至测试服务器 -> 远程调用测试服务器部署脚本

    这里由于CI服务器与测试服务器之间可以内网传输，所以直接使用SCP进行文件传输。

    下面需要一定shell的基础（没有也没关系，可以度娘或google一点点写出来）。

## 下面进入正题

### 安装/启动jenkins
    
    下面的操作以在本人机器上安装为例（MacBook Pro）。
    
    首先到Jenkins官网下载[Jenkins](https://jenkins.io/download/)包。然后还需要下载一个jdk8用于启动Jenkins。

    下面假设环境以及Jenkins都已准备好。打开终端：
    ```
    // 这里jenkins是我的桌面放置jenkins.war文件的目录
    cd ~/Desktop/jenkins

    // 启动jenkins，配置jenkins的操作页面端口。
    java -jar jenkins.war --httpPort=10086

    // 然后访问http://localhost:10086
    // ...
    // ...
    ```

### jenkins配置
    一开始进入jenkin的操作页面会让你注册用户，并需要你访问本机一个文件，将其中的密钥复制出来并粘贴至页面中。
    
    下面越过上述部分，假设已经注册完用户并登录成功。

    点击页面左侧新建按钮，跳转至新建页面。
    
![](/assets/img/jenkins/jenkins-1.png)
    
    填写要构建CI的项目名称，选择构建一个"自由风格的软件项目"，点击"OK"创建项目。

![](/assets/img/jenkins/jenkins-2.png)

    进入项目的配置页面，下面介绍以git为代码管理仓库的配置。
    
    "源码管理"模块，选择"Git"。"Repository URL"填写项目的git的地址（https类型的），"Credentials"配置你的git账号信息（开始需要添加一个，填写你的git账号、密码）。"Branch Specifier"填写你需要polling的分支。

![](/assets/img/jenkins/jenkins-3.png)

    "构建触发器"模块勾选"GitHub hook trigger for GITScm polling"（启用轮询方式监听）、"Poll SCM",下方日程表我这里使用的是"*/3 * * * *"，表示3分钟轮询一次，具体可查询Poll SCM语法。

![](/assets/img/jenkins/jenkins-4.png)

    然后下面需要填写打包上传的shell脚本命令，"构建" -> "Execute shell"。

![](/assets/img/jenkins/jenkins-5.png)

    构建后的操作我这里新增了一个构建失败发送构建失败邮件，这里需要配置邮件服务（由于这里工作不是由我来做的，所以不做赘述。有兴趣的同学可以自行搜索）。

![](/assets/img/jenkins/jenkins-6.png)

## 结尾

通过这次的Jenkins的CI搭建还是学习到了不少新的姿势，其实这只能算是一个简易的CI配置，可优化处还有很多。比如可以使用git hooks，不过在搭建CI服务器之后日常的工作效率确实有了不少的提高。

这篇文章只是对Jenkins搭建前端CI的一个基础的讲解，如果对CI本身不是很了解的话，可以参考[阮一峰大神的介绍](http://www.ruanyifeng.com/blog/2015/09/continuous-integration.html)。


