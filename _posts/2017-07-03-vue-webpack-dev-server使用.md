---
layout: post
title:  "webpack-dev-server使用"
date:   2017-07-03
excerpt: ""
tag:
- vue
comments: true
---

## 安装webpack-dev-server
    
    npm install webpack-dev-server --save-dev

## 启动webpack-dev-server
    
    /**
     * inline：将webpack-dev-sever的客户端入口添加到包(bundle)中。与iframe模式做区分。
     * hot：启用热更新
     * port：指定端口
     * host：指定host访问，（未指定时默认使用127.0.0.1）
     */
    webpack-dev-server --inline --hot --port 10011

>这里需要注意一点，如果webpack-dev-server不是全局安装，在使用webpack-dev-server时会提示webpack-dev-server command not found。解决这个问题很简单，只要使用npm的命令配置就可以了。在package.json中的scripts属性下面添加
    "start": "webpack-dev-server --inline --hot --port 10011"
启动服务使用npm start命令。具体原因是因为没有在node环境下执行webpack-dev-server，而npm启动时会默认开启一个node服务环境，所以问题就解决掉了。

## webpack-dev-server proxy配置

    devServer: {
        historyApiFallback: true,   //
        stats: { colors: true },    //控制台文字颜色
        inline: true,   //这里配置启动命令可以不添加--inline
        hot: true,      //这里配置启动命令可以添加--hot
        proxy: [{
            context: ['/api'],  //这里会正则匹配window.location.pathname
            target: "http://123.56.135.63", //目标地址
            changeOrigin: true, //这里应该是会做origin的改变，但是没效果
            secure: false   //启动ssl
        }]
    }

>这里遇到一个问题，当代理转发至目标服务器时。由于request.Headers.Host没有被更改，还是我网站的host。导致目标服务器配置的ng无法找到反向代理路径，最终导致404 not found，希望有大神可以为我解答，如何解决。

