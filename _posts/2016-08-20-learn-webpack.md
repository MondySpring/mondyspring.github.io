---
layout: post
color: deep-purple
cover: "http://s3-ap-southeast-1.amazonaws.com/monster-machine/images/horssghonr-1436272011-Midas.jpg"
title:  "Webpack!"
date:   2016-08-20 13:50:39
categories: webpack

---

1. webpack的认识

    >webpack是一个模块打包工具,可以把各种文件(React、babel、less、sass等)等模块进行编译打包
    
2. webpack的安装
    >webpack首先在全局环境下进行安装。
    
    `npm install webpack -g`
    
    >或者在常规项目目录下把webpack以来加入到package.json
    
    `npm init` `npm install webpack --save-dev`
3. 项目创建
    安装好之后创建一个learn-webpack的项目进入该文件夹,项目结构如下:
    
        learn-webpack
        | - dist
        | - src
            | - index.js
            | - index.html
            | - style.css
            | - demo.png(任意一张图片)
        | - package.json
        | - webpack.congfig.js
    
    首先看`index.html`代码
    
        <!doctype html>
        <html lang ="en">
        <head>
            <meta charset="utf-8">
            <title>demo</title>
        </head>
        <body>
            <div>hello,world</div>
            <img src="./demo.png>
            <script src="../dist/bundle.js"></script>
        </body>
        </html>
        
    再看`style.css`
    
        body{background:#333;}
      
    此时打开`index.html`,会看到![image](./image/demo.png)
    
    因此,需要对`webpack.config.js`进行配置
    
        //webpack.config.js
        var path = require('path');
        var webpack = require('webpack');
        
        module.exports = {
            entry: './src/index',
            output: {
                path: path.join(__dirname,'dist'),
                filename: 'bundle.js'
            },
            plugins: [
                new webpack.optimize.UglifyJsPlugin({//压缩代码
                    compressor: {
                        warnings: false
                    }
                )}
            ],
            module: {
                loaders: [{
                    test: /\.css$/,
                    loader: 'style!css'
                },
                {
                    test:/\.(png|jpg)$/,
                    loader: [
                        'file?hash=sha512&digest=hex&name=[hash].[ext]',
                        'image-webpack?bypassOnDebug&optimizationLevel=7&interlaced=false'
                    ]
                }]
            }
        }
        
    >以上"__dirname"是node.js中的一个全局变量,只想当前执行脚本所在的目录
    
    >关于loaders,webpack通过外部脚本或工具可以对各种格式的文件进行处理,使用时需要对module进行配置
    
          1. test:一个匹配loaders需哦处理文件的扩张名的正则表达式
          2. loader: loader名称
          3. include/exclude: 手动添加必须处理的文件或疲敝不需要处理的文件
          4. query: 为loader提供额外的设置选项
    
    >使用之前需要安装必备的模块
    
        `npm install style-loader css-loader image-webpack-loader webpack --save-dev`
        
    >使用插件需要npm安装,weback提供一部分插件,然后在webpack配置中的plugins关键在部分添加插件的一个实例
    外部插件需要在webpack.config.js中引入 
    
        var HtmlWebpackPlugin = require('html-webpack-plugin')
    
    最后在index.js中添加 `require('./style.css')` `require('./demo.png')`
    
    运行`webpack`,打开浏览器,就可以看到效果
    
4. 关于webpack    
    
    >webpack有同步和异步加载方式
    加载器loader可以整合资源到Js文件中,形成一个模块,而且支持CommonJs和AMD规范,并且可以自定义webpack的配置
    
    >