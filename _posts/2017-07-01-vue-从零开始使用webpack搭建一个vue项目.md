---
layout: post
title:  "尝试从零开始使用webpack搭建一个vue项目(一)"
date:   2017-07-01
excerpt: ""
tag:
- vue
comments: true
---

## 1. 安装nodejs和npm
webpack需要node环境支持

## 2. 构建项目目录
    project/
        |-- dist/   webpack生成文件存放目录
        |-- static/ 部分静态资源存放目录
        |-- src/    项目源码部分
            |-- assets/ 项目图片资源目录
            |-- components/ 公共组件
            |-- router/ 路由
            |-- main.js 入口js文件
        |-- mock/   模拟数据存放目录
        |-- index.html 首页

## 3. 初始化package.json
    npm init

## 4. 安装各种插件、工具
    
    // 插件会持续更新
    npm install webpack --save-dev
    npm install vue --save

    /**
     * babel-loader：配合webpack使用
     * babel-core：将高版本js语法转换成低版本兼容语法
     * babel-preset-es2015：将es6特性打包
     * babel-plugin-transform-runtime：对es6API部分进行转换
     */
    npm install babel-loader babel-core babel-preset-es2015 babel-plugin-transform-runtime --save-dev
    
    // 将.vue文件转换为.js文件
    npm install vue-loader --save-dev

    npm install vue-style-loader --save-dev

    npm install css-loader --save-dev

    npm install vue-template-compiler --save-dev


## 5. 配置
### 1. babel配置
    // 可以手动创建.babelrc文件，写入配置
    $ touch .babelrc
    $ vi .babelrc

    //.babelrc
    {
      "presets": ["es2015"]
    }

### 2. webpack配置
    $ touch webpack.config.js
    
    // webpack.config.js
    module.exports = {
        entry: __dirname + '/src/main.js', //入口js文件
        output: {
            path: __dirname + '/dist', //输出目录
            filename: 'build.js' //打包之后js文件名称
        },
        module: {
            loaders: [{
                // babel-loader配置
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel-loader'
            }, {
                // vue-loader配置
                test: /\.vue$/,
                loader: 'vue-loader'
            }]
        },
        resolve: {
            // 默认指向vue.common.js，但是需要使用template属性需要compiler.js，vue.js = vue.common.js + compiler.js
            alias: { 'vue$': 'vue/dist/vue.js', }
        }
    };
    
## 结尾
好了，到这里一个简单的vue项目就可以正式开始使用了。
下一章会介绍如何使用webpack-dev-server构建一个本地web服务器。
