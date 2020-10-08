---
title: node建立服务器-express-增删改查
date: 2020-10-08 19:52:29
tags: [node-express,node入门,node框架]
category:
---
### 首先启动项目安装必要的文件：express，fs，art-template,express-art-template,body-parser

## 安装：
```js
npm i --save express express-art-template art-template body-parser
```

## 文件目录
 - views => html文件的 定义那个访问那个，非公开
 - public => 公开的静态文件

## express
```js
// 导入
const  express = require('express')
// 实例化
const app = express()
// 公开目录
app.use("/public/",express.static('./public/'))

// 最后
app.listen(3000,()=>{
    console.log('......')
})
```

## express-art-template 实例化之后
```js
// 模板引擎
app.engine('html', require('express-art-template'));
```

## body-parser 实例化之后
```js
// 解析post * 挂载路之前配置
app.use(bodyParser.urlencoded({ extended: false }))
// parse application/json
app.use(bodyParser.json())
```

## router路由，只关注请求和渲染页面
```js
// 导入只关注处理数据模块 - 增删改查
const zsgc = require('./zsgc')
const express = require('express')

const router = express.Router()

//查看
router.get('/',(req,res) => {
    let students = zsgc.find()
    /**
     * 返回的是json的接口信息，
     * http://localhost:3000/可以直接进行调用
     * res.send(students)
     * return
     * */
    res.render('index.html',{students})
})

//增加 - 页面
router.get('/students/new',(req,res)=>{
    res.render('new.html')
})

//增加功能
router.post('/students/new',(req,res)=>{
    let students = zsgc.find()
    let max = students[0].id
    for(let i = 0;i<students.length;i++){
        students[i].id > max ? max = students[i] : max
    }
    let id = max*1 + 1
    let body = req.body
    body.id = id
    zsgc.save(body)
    res.redirect('/')
})

// 修改 - 页面
router.get('/students/edit',(req,res)=>{
    let students = zsgc.find()
    let id = req.query.id
    let obj = students.find(item => {
        return item.id == id
    })
    res.render('edit.html',{student:obj})
})

// 修改功能
router.post('/students/edit',(req,res)=>{
    let body = req.body
    body.id = body.id*1
    console.log(body)
    let data = zsgc.updata(body)
    res.redirect('/')
})

// 删除
router.get('/students/delete',(req,res) => {
    let id = req.query.id
    zsgc.delete(id*1)
    res.redirect('/')
})

module.exports = router
```

## crud 增删改查，只关注处理文件数据
```js
const fs = require('fs')

let dbPath = './db.json'

// 读出文件 格式化读出的文件
let data = fs.readFileSync(dbPath,'utf8')
let student = JSON.parse(data)

// 保存写入文件
function write(student){
    let obj = JSON.stringify(student)
    fs.writeFileSync(dbPath,obj)
}

//查看
exports.find = ()=>{
    return student.students
}

//增加
exports.save = (data)=>{
    student.students.unshift(data)
    write(student)
}

//修改
exports.updata = (data)=>{
    let index = student.students.findIndex(item => {
        return item.id == data.id
    })
    student.students[index] = data
    write(student)
}

//删除
exports.delete = (id) =>{
   let index =  student.students.findIndex(item => {
        return  item.id == id
    })
    student.students.splice(index,1)
    write(student)
}
```



