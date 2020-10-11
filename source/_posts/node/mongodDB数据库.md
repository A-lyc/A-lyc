---
title: mongodDB数据库
date: 2020-10-11 09:43:18
tags: [数据库,node,mongoDB,node入门]
category: node入门
---

## 安装 
- 网址：https://www.mongodb.com/try/download/community
- 下载社区级别的，免费 也就是本地的第二个
- 根绝菜鸟教程进行安装 网址：https://www.runoob.com/mongodb/mongodb-window-install.html
- 环境变量配置，把mongod的路径给到path添加上即可 我的电脑右键属性。。。。。
- mongod --version  => 查看是否安装很成功

## 启动
- 打开控制台输入 mongod
 · mongod 默认使用执行mongod命令所处盘符根目录下的/data/db作为自己的数据存储
 · 第一次执行mongod时候需要在根盘符创建/data/db文件夹
 · 在那个盘符输入，需要在哪一个盘符中创建一个/data/db 文件夹
 · 修改默认的储存目录
    ```
    mongod --dbpath-数据存储目录路径
    ```
  
  
## 停止
- 开启服务的控制台ctrl+c，或者直接关闭

## 连接数据库和退出
- 连接之前确保开启数据库
- 重新打开控制台 
 · 使用命令mongo 默认打开本机的数据库mongod服务
- 退出
 · exit 在连接状态输入exit 就是退出连接
 
 
## 基本命令
- show dbs
 · 查看当前数据库列表
- db
 · 查看当前操作的数据库
- use 数据库名称 
 · 切换到指定的数据库（如果没有会新建,没有数据show dbs命令显示不出来）
- db.student.insertOne({'name':'syc'})
 · 插入数据库 名称为student 插入数据
- show collections
 · 查看数据库名称
- db.student.find()
 · 查看数据库内容
- db.dropDatabase()
 · 删除数据库
- db.createCollection(name, options)
 · 创建集合
- db.名称.drop()
 · 删除集合 在show collections查看之后删出
 
## 在Node中操作mongod数据库
- 使用官方的mongod包来操作
 · 网址gethup：https://github.com/mongodb/node-mongodb-native

## 使用第三方mongoose框架来操作mongoDB操作数据库
 - 第三方包，基于mongoDB官方第三方包做了封装，名字交mongoose
   · 官网：http://www.mongoosejs.net/
 - 安装使用
  · npm i --save mongoose
  ```js
// 加载包
const mongoose = require('mongoose');
// 连接mongoDB数据库
mongoose.connect('mongodb://localhost/test',{useMongoClient:true});

// 创建一个模型，设计数据库
// mongoDB时动态的，非常灵活，只需要在代码中设计你的数据库就可以了
// mongoose这个包就可以让你的设计编写过程变得简单
// 第一个参数：表名(生成小写复数表明)，第二个参数：数据结构 表中存储的文档，有个name时字符串格式
const Cat = mongoose.model('Cat', { name: String });

// 实例化一个Cat
const kitty = new Cat({ name: '四叶草' + i });

// 持久化保存kitty实例
kitty.save().then(() => console.log('meow,成功')).catch(e=>{console.log(e)});
  ```
- 查看是否存进去了
 · 数据库的名叫test
 · use test中去
 · show collections 查看数据库名
 · db.cats.find()
- 问题 如何让在数据库中出来到最初始目录

 
 
## mongoDB的基本概念
- 多个数据库 - 最外层的对象是数据库
- 一个数据库中可以有多个集合 - 是里面对象中的数组（user:[]）
- 一个集合当中可以有多个文档 - 是集合里面的每一个对象 
- 文档结构没有任何限制
- mongodb多个数据库表
```json
{
    QQ:{
     users:[
      {dom:"文档"},{},{}...
    ],
     products:[],
      ...
    },
    taobao:{
    
    },
    baidu:{
    
    },
}
```

## 官方指南
### 设计Scheme，发布model
```js
const mongoose = require('mongoose');
//拿到mongoose的架构
const { Schema } = mongoose;

// 1 连接mongoDB数据库
mongoose.connect('mongodb://localhost/test');

// 2 设计文档结构 （表结构）
// 就是常见的js数据类型 就是属性名称
// 要求每个文档是下面的类型  Date：是日期  default: Date.now： 默认值
const userSchema = new Schema({
    title:  String,
    email:{
        type:String
    },// 名字是title，类型String 可以不填写
    userName:{
        type:String,// 类型
        required:true // 必选项
    },
    password:{
        type:String,// 类型
        required:true // 必选项
    }
});

// 3 将文档结构发布为模型 - 第一个参数为字符串，第二个为表架构
// mongoose.model用来将一个架构发布为model
// 第一个参数：传入一个大写名词但数字字符串表示你的数据库名称
//            mongoose会将自动大写名词的字符串生成小写复数集合名称
//            举例：这里的User会变成users集合名称
// 第二个参数：是架构 Schema
// 返回值：模型构造函数
const User = mongoose.model('User', userSchema);

// 4 当有了模型构造函数之后，就可以使用这个构造函数对users集合中的数据操作了（增删改查）
```

### 增加数据
```js
// 4 当有了模型构造函数之后，就可以使用这个构造函数对users集合中的数据操作了（增删改查）

let admin = new User({
    userName:"admin",
    password: "12345",
    email: "admin@admin.com",
    title:"user"
})

admin.save().then((ret) => {
    console.log(ret)
}).catch(err => {
    console.log(err)
})

```

### 查询数据
```js
// 查询所有
User.find().then(ret=>{
    console.log('查询')
    console.log(ret)
}).catch(err=> {
    console.log('失败')
})
```
```js
// 按条件查询 - 得到一个数组。查询多个
User.find({userName:'admin'}).then(ret=>{
    console.log('查询')
    console.log(ret)
}).catch(err=> {
    console.log('失败')
})
```
```js
// 得到一个对象。直接找到第一个直接返回，没有返回第一个
User.findOne({userName:'张三'}).then(ret=>{
    console.log('查询')
    console.log(ret)
}).catch(err=> {
    console.log('失败')
})
```

### 删除数据
```js
// 删除数据
User.remove({_id:'5f82a84a875939195ceff0e3'}).then(ret=>{
    console.log('---- 1 ----')
    console.log(ret)
}).catch(err=>{
    console.log('---- 2 ----')
    console.log(err)
})
```

### 更新数据
```js
User.findByIdAndUpdate('5f82a8741c097b40345a29f6',{
    password: '1111'
}).then(ret=>{
    console.log(ret)
}).catch(err=>{
    console.log('shibai')
})
```
