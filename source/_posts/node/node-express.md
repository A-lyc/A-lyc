---
title: node-express
date: 2020-09-27 20:17:11
tags: node-express node框架
category: node-express
---
express官网：https://www.expressjs.com.cn/starter/installing.html
<!-- more -->
###初始化
 - npm init
### 安装
 - npm install express --save
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

### 输出请求url - get请求
 ```js

//根据fs文件找查输出res.end(data)文档显示输出send，我输出结果是下载，之后输出的end
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


