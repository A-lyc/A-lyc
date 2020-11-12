---
title: node基本插件
date: 2020-10-19 21:35:13
tags: [node,node服务器，node插件]
category: node
---
## express node框架可查官网
- 官网：https://www.expressjs.com.cn/starter/installing.html
```shell
npm i --save express
```
## art-template 模板插件
- 官网：https://aui.github.io/art-template/zh-cn/index.html
```shell
npm install --save art-template express-art-template
```
## nodemon 修改自动更新代码
- node自带应该 最好全局安装
```shell
npm i nodemon -g
```
## 解析post请求体的插件
```shell
npm i --save body-parser
```

## 在Node中操作mongod数据库
- 使用官方的mongod包来操作
 · 网址gethup：https://github.com/mongodb/node-mongodb-native

## 使用第三方mongoose框架来操作mongoDB操作数据库
 - 第三方包，基于mongoDB官方第三方包做了封装，名字交mongoose
   · 官网：http://www.mongoosejs.net/
 - 安装使用
```shell
npm i --save mongoose
```

## 图片批处理插件 images
首先先安装npm install images 
- 文件夹：models内建立一个上传图片插件文件夹例如名字：uploadimg
- uploadimg.js
```shell
// 导入可接收图片的插件
const multer = require('multer')
// 定义图片中间件的内容
const storage = multer.diskStorage({
  // 定义保存图片的地址
  destination: function (req, file, cb) {
    cb(null, __dirname + '/../uploads')
  },
  // 定义保存图片的名称，默认没有后缀名，需要添加后缀名
  filename: function (req, file, cb) {
    let mimetype = file.mimetype.split('/')[1]
    cb(null, file.fieldname + '-' + Date.now() + '.' + mimetype)
  }
})
module.exports = multer({ storage })
```
之后在router路由文件内写上传的图片批处理的过程
内有一个批量压缩的文件，可以直接去除
```shell

// 导入express
const express = require('express')

// 导入文件系统
const fs = require('fs')

// 导入路径
const path = require('path')

// 压缩图片插件
var images = require("images");

// 导入压缩图片 - 批量压缩图片
const imageCompress = require('../img.js')

// 使用Router
const router = express.Router()

// 导入上面的图片压缩插件
const upload = require('../models/uploadimg')

 // 上传图片并进行压缩 upload.single('files') - 必选
router.post('/', upload.single('files'), async (req, res) => {
  try {
    let file = req.file
    console.log(file)
    // 返回的线上地址
    file.url = `http://localhost:3000/uploads/${file.filename}`
    /**
    *压缩图片的比例
    * 上传之后直接进行压缩
    *file.path：图片路径
    *size：图片宽高
    *save：（保存路径 - quality：保存质量）
    */
    images(file.path).size(500).save(file.path, { quality: 90})

    // 返回json数据
    res.json({
      code: 20000,
      data: file,// 返回数据的名称
    })
  } catch (e) {
    console.log(e)
  }
})

// 删除图片
router.delete('/', async (req, res) => {
  try {
    // 传来的数据
    let body = req.body
    // 传来的名称
    let name = body.filename
    // 使用node的fs删除当前图片，可以直接传路径
    fs.unlinkSync(path.join(__dirname, '../uploads/' + name))
    // 返回数据
    res.json({
      code: 20000,
      data: [],// 返回数据的名称
      message: '文件已删除',// 返回数据的名称
    })
  } catch (e) {
    console.log('错误')
    console.log(e)
  }
})

// 批量压缩图片
router.get('/compress',async (req,res)=>{
  try{
    // 插件时导入的 const imageCompress = require('../img.js') 
    // 需要传入压缩路径
    imageCompress('uploads')
  } catch (e) {
    console.log('错误')
    console.log(e)
  }
})
// 导出这个路由
module.exports = router;
```
批量压缩图片
```shell
const images = require("images");
const fs = require("fs");
// 导入路径
const WithPath = require('path')
// 需要传入一个参数是文件目录
const path = "uploads"; 
// 压缩后的文件夹，是否保存源文件的问题
const outpath = "compress"; 
module.exports = function compress(path) {
  // 操作文件 读取目录的内容
  fs.readdir(path, function (err, files) {
    if (err) {
      console.log('error:\n' + err);
      return;
    }
    // 循环遍历当前文件目录
    files.forEach(function (file) {
      fs.stat(WithPath.join(__dirname, path + '/' + file), function (err, stat) {
        if (err) { console.log(err); return; }
        if (stat.isDirectory()) {
          // 如果是文件夹遍历
          compress(path + '/' + file);
        } else {
          //遍历图片
          // console.log(__dirname + '文件名:' + path + '/' + file);
          // console.log(WithPath.join(__dirname, path + '/' + file))
          // 要处理的文件夹
          var name = path + '/' + file;
          // 处理之后另保存的文件夹
          var outName = outpath + '/' + file
            /**
            * 压缩图片的比例
            * name：图片路径
            * size：图片宽高
            * save：（保存路径 - quality：保存质量）
            */
          images(name).size(20).save(name, {
            quality: 82                   
          });
        }
      });
    });
  });
}
```
