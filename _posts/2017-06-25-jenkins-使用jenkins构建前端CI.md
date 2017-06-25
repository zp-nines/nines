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

    // 然后访问http://localhost:10080
    // ...
    // ...
    ```

### jenkins配置
    一开始进入jenkin的操作页面会让你注册用户，并需要你访问本机一个文件，将其中的密钥复制出来并粘贴至页面中。
    
    下面越过上述部分，假设已经注册完用户并登录成功。

    点击页面左侧新建按钮，跳转至新建页面。
    
    ![](/assets/img/jenkins/jenkins-1.pngjenkins-1.png)


