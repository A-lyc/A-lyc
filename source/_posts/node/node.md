---
title: node
date: 2020-09-27 18:32:25
tags: node入门
category: node入门
---
node服务器语言
<!-- more -->
### 常用的api
* 系统模块需要require('fs')
 - 文件操作的api：fs

    //文件夹内的路径
    fs.readdri(‘路径’,(error,files)=>{})
    
    //读取文件
    fs.readFile(‘路径’,(error,data)=> {})
    
    //创建文件 - 第二个参数找官网
    fs.open(‘路径’,’’,(error,fd)=>{})
    
    //查找文件存在不存在
    fs.access(‘路径’,(error)=>{})
    
    //追加文件
    fs.appendFile(‘追加文件’,’追加数据’,error=>{})
    
    //写入文件
    fs.writeFile(‘路径’,’内容’,reeoe=>{})
    
    //文件信息
    fs.stats(‘路径’,(error,stats)=>{})
-----

 - 服务器端的api：http
  ```js
//创建启动一个服务器
  http.createServer((req,res)=>{
    let url = req.url//获取到当前url
    res.setHeader('','')//可设置header头，网络请求的res内的应该都可以设置
     /**
     * 点击提交的时候重定向一下，
     * 重定向装态码是302，
     * 重定向到哪里，Location ， /
     * 由于状态码设置了302，所以去找状态码location，然后直接跳转 ’/’这个了
     *
     * */
    res.statusCode = 302
    res.setHeader('Location', '/')
    res.end()//结束响应

}).listen(3000,()=>{})
  ```
-----
 - 操作系统的api：os
-----
 - 路径处理的api：path
-----
* 自定义模块需要require('./XXX'),以路径的方式传入
-----
* 第三方模块需要require('fs')
 - url => npm i url --save 
  · 可以结构url
 - art-template => npm i art-template -S
  · 模板引擎 在使用express的时候需要安装express-art-template
 -----
* 导入导出
  - 内部时exports = module.exports 
  - 导出多个，放在对象内 module.exports = {} 都是用这个
  - 导出单个，放在对象内 exports.a = fu 不建议使用
  - 导入自定义模块：require(‘./’)
  - 导入插件模块：require(‘fs’)
-----
### 使用npm创建项目之前需要npm init - 初始化项目
 - npm常用的命令：
 - 升级npm =>  npm i --global npm 
 - 常用的npm 命令
 - npm init  - npm init -y
 - npm inatall => 只下载 简写npm i
 - npm install 包名 --save => 下载并保存package.json文件中 npm i 包 -S
 - npm uninstall 删除包 => 只删除包，依赖项保存 简写：npm un 包
 - npm uninstall --save =>同时删除依赖项 npm un 包 -S
 - npm help =>查看使用帮助帮助
 
 -----
 ### cmd简单的命令
  - cd 目录 
  - cd .. 上一层
  - dir 目录下文件
  - cls 清屏
  - exit 退出
  - del 文件名  => 删除
  - del*.html => 删除.html的文件、
  - md 新建
  - rd 删除
  - ipconfig 本机ip 