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
 ### 文件操作的api：fs
 
    - 文件夹内的路径
    fs.readdri(‘路径’,(error,files)=>{})
    
    - 读取文件 需要toSteing() error：错误信息 / data：文件内容
    fs.readFile(‘路径’,'utf8',(error,data)=> {})
    
    - 创建文件 - 第二个参数找官网
    fs.open(‘路径’,’’,(error,fd)=>{})
    
    - 查找文件存在不存在
    fs.access(‘路径’,(error)=>{})
    
    - 追加文件
    fs.appendFile(‘追加文件’,’追加数据’,error=>{})
    
    - 写入文件 没有此文件直接创建，有的话直接覆盖 1:路径 2:内容 3:错误信息
    fs.writeFile(‘路径’,’内容’,reeoe=>{})
    
    - 文件信息
    fs.stats(‘路径’,(error,stats)=>{})
-----

 ### 服务器端的api：http
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
### 操作系统的api：os

### node的其他成员
 · 在每个模块中，除了require，exporst等相关的api之外，还有两个特殊成员
  * __dirname 动态获取 - 可以用来获取当前文件模块的所属目录的绝对路径，不包含文件名的路径
  * __filename 动态获取 - 可以用来获取当前文件的绝对路径，包含文件名的路径

 ### 路径操作模块：path
 - 专门用来操作路径的
```shell
/**
 * path.join( __dirname,'./public')
 * 会把相对路径修改成绝对路径 __dirname 解释：当前文件夹的/public文件夹，因为__dirname获取当前文件夹，不包括/public，第二个参数就是追加上/public文件夹额
 * 要用绝对路径来执行就是 path.join( __dirname,'./public') 不要使用./ 来找路径，因为./是在执行node的文件内去找文件路径的
 * 在使用到的./或者没有假__driname的目录向上几层执行node，之后会找不到目录
 * 教程解释：文件操作系统中，相对路径设计的就是相对于执行node命令所处的路径
 * */
app.use('/public',express.static(path.join( __dirname,'./public')))
```
 - path.basename('/目录1/目录2/文件.html','.html')
  · 获取给定目录的最后一部分 - 上面返回文件包含后缀（文件.html） 有第二个参数，加入之后不包含后缀名 （文件）
 - path.dirname('/目录1/目录2/目录3.html');
  · 尾部的目录分隔符会被忽略 获取一个路径中的目录部分
 - path.extname('index.html');
  · 方法会返回 path 的扩展名，即 path 的最后一部分中从最后一次出现 .（句点）字符直到字符串结束。 如果在 path 的最后一部分中没有 .，或者如果 path 的基本名称（参见 path.basename()）除了第一个字符以外没有 .，则返回空字符串。
 - path.isAbsolute(path)
  · 方法检测 path 是否为绝对路径。
 - path.parse()
  · 会返回一个对象，其属性表示 path 的有效元素。 尾部的目录分隔符会被忽略
``` 
path.parse('/目录1/目录2/文件.txt');
    // 返回:
    // { root: '/', // 根路径
    //   dir: '/目录1/目录2',// 目录
    //   base: '文件.txt',// 包含后缀的文件名
    //   ext: '.txt',// 后缀名
    //   name: '文件' // 不包含后缀的文件名
    // }
```
 - path.join() 
  · 方法会将所有给定的 path 片段连接到一起（使用平台特定的分隔符作为定界符），然后规范化生成的路径
  · 路径拼接的时候使用
```shell
path.join('/目录1', '目录2', '目录3/目录4', '目录5', '..');
// 返回: '/目录1/目录2/目录3/目录4'

path.join('目录1', {}, '目录2');
// 抛出 'TypeError: Path must be a string. Received {}'
```
  
-----
### 自定义模块需要require('./XXX'),以路径的方式传入
-----
### 第三方模块需要require('fs')
 - 开发人员发不上去的包，插件
 - url => npm i 包 --save 
  · 可以结构url
 - art-template => npm i art-template -S
  · 模板引擎 在使用express的时候需要安装express-art-template
 -----
## 导入导出
  - 内部时exports = module.exports 
  - 导出多个，放在对象内 module.exports = {} 都是用这个
  - 导出单个，放在对象内 exports.a = fu / function (){return {}}
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
 
 -----
 ### 模块表示的‘/’和文件操作系统中的‘/’
  - 如果只有 ‘/’ 两者都是找磁盘跟目录的下的文件
  - 文件操作系统中 路径可以省略 ./
  - 在模块加载中相对路径的 ./ 不可以省略
 
