---
uuid: 079d9568-a151-1e62-1ec2-642c22b833dc
title: px转换单位为vw和rpx的插件
date: 2020-03-25 20:32:56
tags: vue插件 单位转换
category: vue
---
##  vw
### postcss-px-to-viewport，将px单位自动转换成viewport单位，用起来超级简单，{% link postcss-px-to-viewport 文档 https://npm.taobao.org/package/postcss-px-to-viewport [postcss-px-to-viewport 文档] %} 
· 如果不想转换为Px，大写的P就好
<!-- more -->
####  安装  --dev是安装到开发环境中 打包之后的依赖
> npm install postcss-px-to-viewport --save-dev

####  引入vue项目，再postcss.config.js引入
```
module.exports = {

  plugins: {

    autoprefixer: {}

  },

  "postcss-px-to-viewport": {
      viewportWidth: 750,   // 视窗的宽度，对应的是我们设计稿的宽度，一般是750

      viewportHeight: 1334, // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置

      unitPrecision: 3,     // 指定`px`转换为视窗单位值的小数位数

      viewportUnit: "vw",   //指定需要转换成的视窗单位，建议使用vw

      selectorBlackList: ['.ignore','tab-bar','tab-bar-item'],// 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名

      minPixelValue: 1,     // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值

      mediaQuery: false     // 允许在媒体查询中转换`px`
  }

};
```
####  参数解析
```
{
  unitToConvert: 'px'
  viewportWidth: 320,
  unitPrecision: 5,
  propList: ['*'],
  viewportUnit: 'vw',
  fontViewportUnit: 'vw',
  selectorBlackList: [],
  minPixelValue: 1,
  mediaQuery: false,
  replace: true,
  exclude: [],
  landscape: false,
  landscapeUnit: 'vw',
  landscapeWidth: 568
}
```
* unitToConvert  (String) 要转换的单位，默认是'px'
  viewportWidth (Number) viewport的宽度，默认是320，可根据设计稿来，750的设计稿就写750
  unitPrecision  (Number) 指定`px`转换为视窗单位值的小数位数，默认是5
  propList (Array) 指定可以转换的css属性，默认是['*']，代表全部属性进行转换
    精确匹配
    * 代表全部属性
    在字符串前面或者后面用*，如 ['*position*'] 会匹配background-position-y
    用！则该属性排除. 如: ['*', '!letter-spacing']
    Combine the "not" prefix with the other prefixes. Example: ['', '!font']

* viewportUnit  (String)指定需要转换成的视窗单位，默认vw
  fontViewportUnit  (String)指定字体需要转换成的视窗单位，默认vw
  selectorBlackList  (Array) 指定不转换为视窗单位的类，保留px，值为string或正则regexp，建议定义一至两个通用的类名
    值为string类型， 检查字符是否包含
      ['body'] 匹配 .body-class
  值为regexp类型，正则匹配.
      [/^body$/] 匹配 body 而不是 .body

​​​​​​​minPixelValue (Number) 默认值1，小于或等于`1px`不转换为视窗单位,
mediaQuery  (Boolean) 是否在媒体查询时也转换px，默认false
replace (Boolean)  replaces rules containing vw instead of adding fallbacks.
exclude (Array or Regexp) 设置忽略文件，如node_modules
  如果是regexp, 忽略全部匹配文件.
  如果是数组array, 忽略指定文件.

-----
可能遇到的问题
1、@keyframes 和media查询里的px默认是不转化的，设置mediaQuery： true则媒体查询里也会转换px

@keyframes可以暂时手动填写vw单位的转化结果


####  px2rem
如何在vue-cli3.0中使用postcss-plugin-px2rem 插件
插件的作用是 自动将vue项目中的px转换为rem 。

为什么这三个中要推荐 postcss-plugin-px2rem呢？

因为 postcss-plugin-px2rem 这个插件 配置选项上有 exclude 属性，它可以配置 是否对 某个文件夹下的所有css文件不进行从px到rem的转换。

所以我们可以利用这个特性，把项目中的 node_module 文件夹排除掉。这样如果我们项目中是用了，前端UI框架的话，就不会吧UI框架（Vant,Element等）中的 px单位转换成rem了

* postcss-plugin-px2rem 官方文档：https://www.npmjs.com/package/postcss-plugin-px2rem
  postcss-pxtorem 官方文档：https://www.npmjs.com/package/postcss-pxtorem

postcss-px2rem 官方文档：https://www.npmjs.com/package/postcss-px2rem

使用npm安装插件：
npm i postcss-plugin-px2rem  --save -dev

具体配置方法如下：
在vue-cli3.0中。去掉了build和config文件夹。所有的配置都放到了vue.config.js中（默认为空，如果没有这个文件自己写一个）。

反向代理的配置
```
module.exports = {
    //反向代理的配置
    devServer: {
        proxy: {
            '/api': {
                target: 'http://-,//目标地址
                ws: true, //// 是否启用websockets
                changeOrigin: true, //开启代理：在本地会创建一个虚拟服务端，然后发送请求的数据，并同时接收请求的数据，这样服务端和服务端进行数据的交互就不会有跨域问题
                pathRewrite: {'^/api': '/'}    //这里重写路径
            }

        }
    },
}

```

在vue-cli3中使用postcss-px2rem 配置类似，如下：

1. 使用postcss-px2rem时的vue.config.js配置：
```
module.exports = {
    lintOnSave: true,
    css: {
        loaderOptions: {
            postcss: {
                plugins: [
                    require('postcss-px2rem')({ //配置项，详见官方文档
                        remUnit: 30
                    }), // 换算的基数
                ]
            }
        }
    },
}            
```
记得npm i 安装包；

可能遇到的坑：

如果个别地方不想转化px。可以简单的使用大写的 PX 或 Px 。