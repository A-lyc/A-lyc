---
title: node基本插件
date: 2022-01-19 21:35:13
tags: [node,node服务器，node插件]
category: node
---

## 后端：Strapi 非常好的后端框架
官网：https://strapi.io/
- 使用npm
```shell
npx create-strapi-app my-project --quickstart
```

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

## 处理excel文件的读写
### 一、读取
#### 1. 安装 node-xlsx
```shell
   npm install node-xlsx --save
```

#### 2. 解析代码
```shell
const xlsx = require('node-xlsx');
 
// 解析得到文档中的所有 sheet
let sheets = xlsx.parse('xxx.xls');
 
// 遍历 sheet
sheets.forEach(function(sheet){
    console.log(sheet['name']);
    // 读取每行内容
    for(let rowId in sheet['data']){
        console.log(rowId);
        let row=sheet['data'][rowId];
        console.log(row);
    }
});
```
### 二、写入
```shell
var data = [
    {
        name: 'sheet1',
        data: [
            [
                'ID',
                'Name',
                'Score'
            ],
            [
                '1',
                'Michael',
                '99'

            ],
            [
                '2',
                'Jordan',
                '98'
            ]
        ]
    },
    {
        name: 'sheet2',
        data: [
            [
                'AA',
                'BB'
            ],
            [
                '23',
                '24'
            ]
        ]
    }
]
var buffer = xlsx.build(data);

// 写入文件
fs.writeFile('a.xlsx', buffer, function(err) {
    if (err) {
        console.log("Write failed: " + err);
        return;
    }

    console.log("Write completed.");
});
```

## 文件解压
### 安装node-stream-zip
```shell
npm i --save node-stream-zip
```
新建文件夹为fileType内有files-1605681955440.zip文件  
同级别的新建unapi.js,代码如下，
这个是导出了一个Promise
```shell
// 导入压缩包
const StreamZip = require('node-stream-zip');

// 导入文件模块
const fs = require('fs');


// 插件字写 格式化时间
const { getNowFormatDate } = require('./common/dateTime')
let time = getNowFormatDate('')

// 手动删除压缩包

/**
 * path:传入路径压缩包的路径，包括文件夹和文件名
 * 
 */
module.exports = function unzipCompress(path,) {
  // 文件路径，固定的
  let filesType = './fileType/'

  // 根据路径裁切出文件名
  // let name = path.split(filesType)[1]

  // 查找 某个文件 测试使用
  const zip = new StreamZip({
    file: './fileType/files-1605681955440.zip',
    storeEntries: true
  });
  // 返回一个promise
  return new Promise((res,rej)=> {
    // 错误的时候
    zip.on('error', err => {
      rej(err)
    });
    // 返回一个数组，但是会少去一个文件
    let arr = []

    // 在解压文件夹时，您可以侦听解压事件 - 图片重命名
    zip.on('extract', (entry, file) => {
      console.log(`Extracted ${entry.name} to ${file}`);
      let name = file.split('.')[1]
      /**
       * 1：旧文件所在的目录，包括文件名，2：新文件所在的目录，包括文件名，3：回调
       */
      fs.rename(`${file}`, `./fileType/files-${Date.now()}${time}.${name}`, err => {
        if (err) throw err;
        console.log('重命名完成')
        arr.push(`./fileType/files-${Date.now()}${time}.${name}`)
      })
    });

    // 提取所有
    zip.on('ready', () => {
      // 创建目录
      // fs.mkdirSync('extracted');
      // 1：unll ，2：文件路径， 3：回调函数
      zip.extract(null, './fileType', (err, count) => {
        console.log(err ? 'Extract error' : `Extracted ${count} entries`);
        res(arr)
        zip.close();
      });
    });

    // 在加载期间为每个条目生成条目事件
    zip.on('entry', entry => {
      console.log('在加载期间为每个条目生成条目事件')
      // you can already stream this entry,
      // without waiting until all entry descriptions are read (suitable for very large archives)
      console.log(`XXX ：  ${entry.name.split('.')[1]} + ./fileType/${entry.name}`);
    });
  })
}



```
### 新建文件使用
```shell
const xlsxCompress = require('./xlsx');
async function name(params) {
  xlsxCompress().then(res => {
   console.log('---- 1 ----')
   console.log(res)
   console.log('---- 2 ----')
 })
  
} 
name()
```

## 图片批处理插件 images
### 首先先安装npm install images 
文件夹：models内建立一个上传图片插件文件夹例如名字：uploadimg
uploadimg.js
```shell
// 导入可接收图片的插件
const multer = require('multer')

const fs = require('fs')

// 插件字写 格式化时间
const { getNowFormatDate } = require('../common/dateTime')
let time = getNowFormatDate('')

// 定义图片中间件的内容
const storage = multer.diskStorage({
  // 定义保存图片的地址
  destination: function (req, file, cb) {
    // 某个文件夹下的日期文件下的图片目录
    let path = __dirname + '/../uploads/' + time
    // 返回这个目录
    file.time = time
    let ifFile = fs.existsSync(path)
    
    if (ifFile){
      cb(null, path)
    }else{
      fs.mkdirSync(path)
      cb(null, path)
    }
  },

  // 定义保存图片的名称，默认没有后缀名，需要添加后缀名
  filename: function (req, file, cb) {
    let suffix = file.originalname.split('.')
    suffix = suffix[suffix.length - 1]
    // console.log(suffix)

    // 图片使用
    // let mimetype = file.mimetype.split('/')[1]
    cb(null, file.fieldname + '-' + Date.now() + time + '.' + suffix)
  }
})
module.exports = multer({ storage })
```
### router
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
### 批量压缩图片 - img.js
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
          });            quality: 82                   

        }
      });
    });
  });
}
```
### 附注：api接口
images(file)
Load and decode image from file
从指定文件加载并解码图像

images(width, height)
Create a new transparent image
创建一个指定宽高的透明图像

images(buffer[, start[, end]])
Load and decode image from a buffer
从Buffer数据中解码图像

images(image[, x, y, width, height])
Copy from another image
从另一个图像中复制区域来创建图像

.fill(red, green, blue[, alpha])
eg:images(200, 100).fill(0xff, 0x00, 0x00, 0.5) Fill image with color
以指定颜色填充图像

.draw(image, x, y)
Draw image on the current image position( x , y )
在当前图像( x , y )上绘制 image 图像

.encode(type[, config])
eg:images("input.png").encode("jpg", {operation:50}) Encode image to buffer, config is image setting.
以指定格式编码当前图像到Buffer，config为图片设置，目前支持设置JPG图像质量
Return buffer
返回填充好的Buffer
Note:The operation will cut off the chain
注意:该操作将会切断调用链
See:.save(file[, type[, config]]) 参考:.save(file[, type[, config]])

.save(file[, type[, config]])
eg:images("input.png").encode("output.jpg", {operation:50}) Encoding and save the current image to a file, if the type is not specified, type well be automatically determined according to the file, config is image setting. eg: { operation:50 }
编码并保存当前图像到 file ,如果type未指定,则根据 file 自动判断文件类型，config为图片设置，目前支持设置JPG图像质量

.size([width[, height]])
Get size of the image or set the size of the image,if the height is not specified, then scaling based on the current width and height
获取或者设置图像宽高，如果height未指定，则根据当前宽高等比缩放

.resize(width[, height])
Set the size of the image,if the height is not specified, then scaling based on the current width and height
设置图像宽高，如果height未指定，则根据当前宽高等比缩放, 默认采用 bicubic 算法。

.width([width])
Get width for the image or set width of the image
获取或设置图像宽度

.height([height])
Get height for the image or set height of the image
获取或设置图像高度

images.setLimit(width, height)
Set the limit size of each image
设置库处理图片的大小限制,设置后对所有新的操作生效(如果超限则抛出异常)

images.setGCThreshold(value)
Set the garbage collection threshold
设置图像处理库自动gc的阈值(当新增内存使用超过该阈值时，执行垃圾回收)

images.getUsedMemory()
Get used memory (in bytes)
得到图像处理库占用的内存大小(单位为字节)

images.gc()
Forced garbage collection
强制调用V8的垃圾回收机制

## node的实时通讯socket.io
```shell
npm i --save socket.io
```
需要研究一下 。。。。。。

## pdf-lib 模块可让你处理PDF，
后台文件的functionLife中的psfLid.js中

## sharp 可让您处理几乎所有带有图像的东西，
暂时为使用。。。。。。

## pkg 将Node项目捆绑到独立的可执行文件中
暂时为使用。。。。。。

## qrcode生成二维码
前端直接使用，之后把图传给后端，或者前端把需要生成的东西传给后端后端进行生成之后返回图片
```shell
let canvas = this.$refs['canvas']
  QRCode.toCanvas(canvas, 'sample text', function (error) {
    if (error) console.error(error)
    console.log('success!')
})
```

## 百度图表
安装
```shell
npm install echarts --save
```
简单使用：
```shell
// 此方法是挂载dom之后
 mounted() {
    // 图表 - 需要页面挂在之后完成
    let echart = this.$refs['echarts']
    var myChart = echarts.init(echart)
    var option = {
      legend: {},
      tooltip: {},
      dataset: {
        // 提供一份数据。
        source: [
          ['product', '2015', '2016', '2017', '2018'],
          ['Matcha', 43.3, 85.8, 93.7],
          ['Milk', 83.1, 73.4, 55.1],
          ['Cheese', 86.4, 65.2, 82.5],
          ['W', 72.4, 53.9, 39.1],
          ['Wal=', 70, 1, 90,100],
          ['Waln', 70, 1, 90,100],
          ['Walnu', 70, 1, 90,100],
          ['Walnut', 70, 1, 90,100],
        ]
      },
      // 声明一个 X 轴，类目轴（category）。默认情况下，
      // 类目轴对应到 dataset 第一列。
      xAxis: {type: 'category'},
      // 声明一个 Y 轴，数值轴。
      yAxis: {},
      // 声明多个 bar 系列，默认情况下，每个系列会自动对应到 dataset 的每一列。
      series: [
        {type: 'bar'},
        {type: 'bar'},
        {type: 'bar'},
        {type: 'bar'},
      ]
    }
    // 使用刚指定的配置项和数据显示图表。
    myChart.setOption(option)
  }
}
```

