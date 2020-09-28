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
 
### 路由
```app.METHOD(PATH, HANDLER)```
 - app是的实例express
 - METHOD是一个HTTP请求方法（小写）
 - PATH 是服务器上的路径
 - HANDLER 是当路由匹配时执行的功能
 
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
 ```app.use('/public/', express.static('./public/'))```
 ```http://localhost:3000/public/images/kitten.jpg```
 
### 处理404和错误处理器
 - 在Express中，404响应不是错误的结果，因此错误处理程序中间件将不会捕获它们。此行为是因为404响应仅表明没有要做的其他工作。换句话说，Express已经执行了所有中间件功能和路由，但发现它们均未响应。您需要做的就是在堆栈的最底部添加一个中间件功能（在所有其他功能下方）以处理404响应：
 ```js
app.use(function (req, res, next) {
      res.status(404).send("Sorry can't find that!")
    })
```
 - 错误处理器中间件的定义和其他中间件一样，唯一的区别是 4 个而不是 3 个参数，即 (err, req, res, next)：
 ```js
app.use(function (err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```

### next 线程处理程序 - 直接上例子
```js
app.get('/example/b', function (req, res, next) {
  console.log('the response will be sent by the next function ...')
  next()
}, function (req, res) {
  res.send('Hello from B!')
})
// 02
var cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

var cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

var cb2 = function (req, res) {
  res.send('Hello from C!')
}

app.get('/example/c', [cb0, cb1, cb2])

//组合
var cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

var cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

app.get('/example/d', [cb0, cb1], function (req, res, next) {
  console.log('the response will be sent by the next function ...')
  next()
}, function (req, res) {
  res.send('Hello from D!')
})
```

### res()对应的相应
<table>
  <thead>
    <tr>
      <th><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">方法</font></font></th>
      <th><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">描述</font></font></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="/en/4x/api.html#res.download"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">res.download()</font></font></a></td>
      <td><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">提示要下载的文件。</font></font></td>
    </tr>
    <tr>
      <td><a href="/en/4x/api.html#res.end"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">res.end()</font></font></a></td>
      <td><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">结束响应过程。</font></font></td>
    </tr>
    <tr>
      <td><a href="/en/4x/api.html#res.json"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">res.json（）</font></font></a></td>
      <td><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">发送JSON响应。</font></font></td>
    </tr>
    <tr>
      <td><a href="/en/4x/api.html#res.jsonp"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">res.jsonp（）</font></font></a></td>
      <td><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">发送具有JSONP支持的JSON响应。</font></font></td>
    </tr>
    <tr>
      <td><a href="/en/4x/api.html#res.redirect"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">res.redirect（）</font></font></a></td>
      <td><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">重定向请求。</font></font></td>
    </tr>
    <tr>
      <td><a href="/en/4x/api.html#res.render"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">res.render（）</font></font></a></td>
      <td><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">渲染视图模板。</font></font></td>
    </tr>
    <tr>
      <td><a href="/en/4x/api.html#res.send"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">res.send（）</font></font></a></td>
      <td><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">发送各种类型的响应。</font></font></td>
    </tr>
    <tr>
      <td><a href="/en/4x/api.html#res.sendFile"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">res.sendFile（）</font></font></a></td>
      <td><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将文件作为八位字节流发送。</font></font></td>
    </tr>
    <tr>
      <td><a href="/en/4x/api.html#res.sendStatus"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">res.sendStatus（）</font></font></a></td>
      <td><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">设置响应状态代码，并将其字符串表示形式发送为响应正文。</font></font></td>
    </tr>
  </tbody>
</table>





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


