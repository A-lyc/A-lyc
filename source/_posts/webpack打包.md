---
uuid: e2c5e0ef-fad1-c4ac-679c-919dea55e60f
title: webpack打包
date: 2020-06-06 22:53:59
tags: webpack打包
category: webpack打包
---
单个文件初始化webpack

首先新建一个src文件，内有js，css，html文件，之后在npm init初始化一下wenpack，安装webpack，之后本地安装webpack：npm install --save-dev webpack，webpack4.0需要安装cli：npm install --save-dev webpack-cl
在scripts内的打包后build:"webpack --config webpack.config.js"



1：单个js：比如将一个js打包到另一个js中：webpack index.js min.js将index.js打包到min.js中
2：多文件需要export导出，import导入景行打包
3：webpack初始化 npm init
4：css的话需要安装加载器，npm i css-loader style-loader
5：安装webpack本地环境运行方式:npm install webpack-dev-serve --save-sev



//单文件
```js
//webpack是node生成的
let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')//自带好像，格式化html的
module.exports = {
  devServer: {//开发服务器配置
    port: 3000,//端口
    progress: true,//进度条
    contentBase: "./dist",//开发指向的目录
    compress: true,//压缩
  },
  mode: 'production',//默认两种，生产模式production和开发模式development
  entry: './src/index.js',// 入口 -> 在哪个位置开始打包
  output: {
    path: path.resolve(__dirname, 'dist'),//选要node模块绝对路径
    filename: 'bundle.[hash:3].js',//出口文件名  【】内是哈希值，取值3位
  },
  plugins: [//插件，放置所有webpack插件
    new HtmlWebpackPlugin({
      template: "./src/index.html",//导出html模板
      filename: 'index.html',//模板名称
      minify: {
        removeAttributeQuotes: true,//删除双引号
        collapseWhitespace: true,//折叠空行
      },
      hash: true//生成哈希值

    })
  ],
  module: {//模块
    rules: [//规则loader
      {//css的loader css-loader 引入@import的语法 //还有个一个是style-loader把css插入header中
        //loader处理单一
        //loader用法，可以是字符串，多个需要数组,对象：如下，可以传输参数
        //loader顺序，默认从右向左
        test: /\.css$/,
        use: [{
          loader: 'style-loader'
        }, 'css-loader']
      }
    ]
  }
}
```