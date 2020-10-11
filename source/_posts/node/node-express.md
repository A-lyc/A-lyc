---
title: node-express
date: 2020-09-27 20:17:11
tags: node-express node框架
category: node-express node入门
---
express官网：https://www.expressjs.com.cn/starter/installing.html
<!-- more -->
###初始化
 - npm init
 
### 安装
 - npm install express --save
 - npm install --save art-template express-art-template
 - 
 
### hello world
```js
const express = require('express')
//创建app
const app = express()

app.get('/',(req,res)=>{
    res.send('你好a ')
})

app.listen(3000,()=>{
    console.log('服务器启动成功')
})
```

### 基本路由
- 请求方法，路径，处理函数
```js
// get 当以get请求的'/'的时候执行后面的函数
app.get('/',(req,res)=>{res.send('hello world get')})

// post 当以post请求的'/'的时候执行后面的函数
app.post('/',(req,res)=>{res.send('hello world post')})
// .....
```

### 静态服务
 - 开放公共资源
```js
// 当以'/public/'开头的时候，去'./public/'这个目录下找文件
// 当没有第一个参数的时候，写的时候直接去除public就好，比如./public/index.js可写index.js
app.use('/public/', express.static('./public/'))
```


### 引入包
 - const express = require('express')
 
### 安装模板插件
 - npm install --save express-art-template art-template
 - 引入和使用
  ```js
    // 后缀名为html 引入包
    app.engine('html', require('express-art-template'));
    //必要的view options
    app.set('view options', {
    debug: process.env.NODE_ENV !== 'production'
    });
    //重新定义路径 可以不写但是文件要是views下的 - 第一个参数不能变 第二个参数事定义的路径
    app.set('views', path.join(__dirname, 'views'));
    //官网有，不知道啥意思
    app.set('view engine', 'art');  
    //使用
    app.get('/', function(req, res) {
        /*
        * 当‘/’时直接输出views中的index.html,
        * 有文件夹的时候直接写文件夹名称 + 文件名称
        * res.render('./home/index.html',{comments})
        * */
    res.render('index.html',{comments})
})
```

### 开放公共资源
 - app.use('/public/', express.static('./public/'))

### 修改晚代码自动重启
 - 使用一个第三方命令行工具nodemon来帮助我们解决频繁修改代码重启服务器问题
  · nodemon - 基于node.js开发的一个命令行工具，使用的时候需要独立安装
  ```
// 全局安装，在任意目录执行
  npm i -global nodemon
```
  · 安装完毕后，使用：
  ```js
    // 原来是 
    node app.js 
    //# 使用nodemon
    nodemon app.js
  ```
  · 只要通过nodemon app.js 启动的时候会自动监听文件的变化

### 输出请求url - get请求
 ```js

//根据fs文件找查输出res.end(data)文档显示输出send，我输出结果是下载（更改html上的Content-Type: text/html; charset=utf-8）或者重新打包输出
//最好使用send(),框架自带的应用
app.get('/', (req, res) => {
    fs.readFile('./views/index.html',(error,data)=>{
        if(error) return res.send('404');
        res.end(data)
    })
})
//根据点击动作输出
app.get('/pinglun', (req, res) => {
    let query = req.query
    query.dateTime =' 2020-09-31'
    comments.unshift(query)
    //重定向
    res.statusCode = 302
    //跳转    
    res.setHeader('Location', '/')
    //结束输出    
    res.end()
})

//使用模板解析插件输出
app.get('/post', function(req, res) {
    /*
    * 内有模板的标识符
    * res.render('index.html',{comments})
    */
    res.render('post.html')
})
```


