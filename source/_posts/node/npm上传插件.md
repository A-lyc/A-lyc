---
title: npm上传插件
date: 2021-09-25 21:35:13
tags: [npm,老虎机,抽奖]
category: npm上传插件
---

<a href='./draw-lottery.zip'>自己写的多循环老虎机插件</a>

## 1、npm是什么
npm 是Node 的模块管理器，功能极其强大。 它是Node 获得成功的重要原因之一。 正因为有了npm，我们只要一行命令，就能安装别人写好的模块。

## 2、项目初始化

1 首先安装好脚手架vue-cli,打开终端输入命令
```shell
$ npm install --global vue-cli #全局安装
```

2 这里封装vue的插件用webpack-simple很合适，webpack-simple类似于webpack。
接着在终端进入需要创建项目的目录下，输入命令

```shell
$ vue init webpack-simple project-name #project-name为项目名，在这我的项目名为vue-mytoast
```
如图：
![查入图片](/1.webp) 
3 其中
"Pproject name":项目名称
“Project description”: 对项目的描述
"Author": 作者
"LIcense":开源协议
"Use sass？"：是否使用sass，这里我们没用到，选择否（No）

4 按照提示输入命令
```shell
$ cd vue-mytoast #进入文件夹
$ npm install #按照依赖包
$ npm run dev #启动项目
```

最终的项目结构：
![查入图片](/2.webp) 

我们在 src目录下创建一个lib文件夹，里面存放需要开发的插件文件，如图
![查入图片](/3.webp) 

## 3、所有环境搭建好后，进入插件开发

1 在lib文件夹下创建一个toast.vue文件，代码如下
```js
<template>
  <div class="vue-toast-wraper" v-show="isShowToast">
    {{toastMsg}}
  </div>
</template>

<script>
    export default {
        name: "vueToast",
        data() {
          return {
            toastMsg: "登陆成功",
            isShowToast:true
          }
        },
      mounted() {
          if (this.isShowToast) {
            setTimeout(() => {
               this.isShowToast = false;
            },2500);
          }
      }
    }
</script>

<style scoped>
  .vue-toast-wraper{
    background: rgba(0, 0, 0, 0.6);
    color: #fff;
    font-size: 17px;
    padding: 10px;
    border-radius:12px;
    display: -webkit-box;
    -webkit-box-pack: center;
    -webkit-box-align: center;
    position: fixed;
    top: 50%;
    left: 50%;
    z-index: 2000;
    -webkit-transform: translateY(-50%);
    transform: translateY(-50%);
    -webkit-transform: translateX(-50%);
    transform: translateX(-50%);
  }
</style>
```

2 创建一个toast.js文件写install方法，代码如下
```js
import Toast from './toast.vue'
const vueToast = {
  install(Vue, options) {
     //Toast.name 组件的name属性,也就是后面使用的组件名字
    Vue.component(Toast.name, Toast)
    // 类似通过  this.$xxx方式调用插件，其实只是挂载到原型上而已
    //Vue.prototype.$xxx //最终可以在任何地方通过 this.$XXX调用
  }
}
//global情况下自动安装
if (typeof  window !== 'undefined' && window.Vue) {
  window.Vue.use(vueToast)
}

export default vueToast
```
3 写好后整个项目基本开发完成了，下面先在本项目测试一下
3.1 在 main.js 文件中全局引入toast.js
![查入图片](/4.webp) 

3.2 然后在App.vue中使用
![查入图片](/5.webp) 
如果在浏览器出现我们开发的组件，则开发成功！


## 4、开发完成后进行文件配置
1 修改package.json文件
```js
{
  "name": "vue-mytoast",
  "description": "vue弹窗提示插件",
  "version": "1.0.0",
  "author": "lwz",
  "license": "MIT",   //开源协议
  "private": false,  //（改） 因为组件包是公用的，所以private为false
  "scripts": {
    "dev": "cross-env NODE_ENV=development webpack-dev-server --open --hot",
    "build": "cross-env NODE_ENV=production webpack --progress --hide-modules"
  },
  "main": "dist/toast.js",  //（改） 配置main结点，如果不配置，我们在其他项目中就不用import XX from '包名'来引用了，只能以包名作为起点来指定相对的路径

  "dependencies": {
    "vue": "^2.5.11"
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not ie <= 8"
  ],
  "keywords": [     //（改） 指定关键字
    "vue",
    "toast"
  ],
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.2",
    "babel-preset-env": "^1.6.0",
    "babel-preset-stage-3": "^6.24.1",
    "cross-env": "^5.0.5",
    "css-loader": "^0.28.7",
    "file-loader": "^1.1.4",
    "vue-loader": "^13.0.5",
    "vue-template-compiler": "^2.4.4",
    "webpack": "^3.6.0",
    "webpack-dev-server": "^2.9.1"
  }
}
```
2 修改webpack.config.js文件
2.1 由于不是所有使用组件的人都是通过 npm 安装使用 import 引入组件的，还有很多人是通过
 <script></script>
 标签的方式直接引入的，所以我们要将 libraryTarget 改为 umd 格式，同时我们要配置文件入口和出口
```js
var path = require('path')
var webpack = require('webpack')

// 执行环境
const NODE_ENV = process.env.NODE_ENV

module.exports = {
  // entry: './src/main.js',
  // 根据不同的执行环境配置不同的入口
  entry: NODE_ENV == 'development' ? './src/main.js' : './src/lib/toast.js',
  output: {
    //修改打包出口，在最外级目录打包出一个 index.js 文件，我们 import 默认会指向这个文件
    path: path.resolve(__dirname, './dist'),
    publicPath: '/dist/',
    // filename: 'build.js'
    filename: "toast.js", //指定最终打包成dist文件夹下的文件名
    library: "vueToast",  // 指定的就是你使用require时的模块名
    libraryTarget: "umd", //libraryTarget会生成不同umd的代码,可以只是commonjs标准的，也可以是指amd标准的，也可以只是通过script标签引入的
    umdNamedDefine: true  //会对 UMD 的构建过程中的 AMD 模块进行命名。否则就使用匿名的 define
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'vue-style-loader',
          'css-loader'
        ],
      },      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          loaders: {
          }
          // other vue-loader options go here
        }
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
      },
      {
        test: /\.(png|jpg|gif|svg)$/,
        loader: 'file-loader',
        options: {
          name: '[name].[ext]?[hash]'
        }
      }
    ]
  },
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    },
    extensions: ['*', '.js', '.vue', '.json']
  },
  devServer: {
    historyApiFallback: true,
    noInfo: true,
    overlay: true
  },
  performance: {
    hints: false
  },
  devtool: '#eval-source-map'
}

if (process.env.NODE_ENV === 'production') {
  module.exports.devtool = '#source-map'
  // http://vue-loader.vuejs.org/en/workflow/production.html
  module.exports.plugins = (module.exports.plugins || []).concat([
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"'
      }
    }),
    new webpack.optimize.UglifyJsPlugin({
      sourceMap: true,
      compress: {
        warnings: false
      }
    }),
    new webpack.LoaderOptionsPlugin({
      minimize: true
    })
  ])
}


```

3 修改.gitignore 文件
4 因为要用dist文件夹，所以在.gitignore文件中把dist/去掉。
```shell
.DS_Store
node_modules/
# dist/
npm-debug.log
yarn-error.log

# Editor directories and files
.idea
*.suo
*.ntvs*
*.njsproj
*.sln
```

5 修改index.js文件

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>vue-mytoast</title>
  </head>
  <body>
    <div id="app"></div>
    <!--<script src="/dist/build.js"></script>-->
    <script src="/dist/toast.js"></script>
  </body>
</html>
```




## 5、配置完毕，进入发布阶段
在发正式包之前可以在本地先打一个包，然后测试下有没有问题，如果没问题再发布到npm上。
1 首先，打包到本地
```shell
$ npm run build 
$ npm pack 
```

2 npm pack 之后，就会在当前目录下生成 一个tgz 的文件。
打开一个新的vue项目，在当前路径下执行(‘路径’ 表示文件所在的位置)
```shell
npm install 路径/组件名称.tgz 
```
3 如图我在project的项目中引入vue-mytoast组件
![查入图片](/6.webp) 

4 然后，在新项目的入口文件（main.js）中引入
```shell
import vueToast from 'toast'
Vue.use(vueToast)
```
5 在组件中使用
```shell
<div>
   <vue-toast></vue-toast>
</div>
```
6 测试没问题，可以发布啦！

7 首先在npm官网上注册一个npm账号
8 然后在发布项目的根目录下输入命令
```shell
$ npm adduser
```
9 输入npm账号，输入用户名、密码、邮箱
![查入图片](/7.webp) 

10 在这里特别说明一点，在npm注册的账号一定要把镜像切换到http://registry.npmjs.org/.中来，如果你使用的是淘宝镜像或者别的镜像请切换到此镜像，否则会报错
命令：
```shell
$ npm config set registry https://registry.npmjs.org/
```
11 登陆成功后进行 npm publish发布
```shell
$ npm publish
```
12 如果报下面这个错，则在邮箱中核实一下发来的邮件即可
13 发布成功后到自己npm上可以看到刚才发的npm包
![查入图片](/8.webp)
14 发布成功后，当别人想使用你的插件时就可以用 npm install 组件名称 来安装了！

## 6、后记
折腾了很久，终于把自己的第一个插件的雏形发布出去了，这是一个很简单的插件，以后会继续维护它，添加更多的实用功能。

1 更新npm包：
```shell
$ npm version   <update_type>;
$ npm publish

使用命令：npm version <update_type>进行修改，update_type 有三个参数，第一个是patch,  第二个是minor,第三个是 major，
  patch：这个是补丁的意思，补丁最合适；
  minor：这个是小修小改；
  major：这个是大改咯；
 具体咋用：
   比如我想来个1.0.1版本，注意，是最后一位修改了增1，那么命令：npm version patch    回车就可以了；
   比如我想来个1.1.0版本，注意，是第二位修改了增1，那么命令： npm version minor    回车就可以了；
   比如我想来个2.0.0版本，注意，是第一位修改了增1，那么命令： npm version major     回车就可以了；
```
npm unpublish --force：移除一个发布包（也可以移除指定版本的包）
npm logout：登出用户



简书作者：lwz4070
原文链接：https://www.jianshu.com/p/b98d09609717
来源：简书