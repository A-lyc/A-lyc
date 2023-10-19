---
title: egg后台开发
date: 2022-02-16 18:32:25
tags: egg后台开发
category: egg后台开发
---
# 安装node，之后安装egg
   · 自己已写完一个egg，基础的增删改查，基础上写了一个后台，可在这个基础之上继续开发，对接数据库的时候很不友好。需要改进和数据库的链接

# egg目录介绍
 主要文件：app.js，config，app
 ## Application - app.js
  - 新建app.js  和app文件夹是同级目录
  在app.js中写入方法
  在Controller 文件中使用的时候
  ```js
  console.log(this.app.cache());
 ```
   ## 入口:app.js
   ```js
    module.exports = app => {
      app.beforeStart(async function(){
        console.log('系统启动的时候执行 会执行这个函数 之后直接创建的数据库的表')
        //  await app.model.sync({force:true})  //每次启动删除全部的表，按照模型创建新的表 开发环境使用，会删除数据表
        // sync方法会根据模型创建一个表
        await app.model.sync({})
      })
      app.cache = () => {
        console.log(app.controller);
        console.log('我是app.js');
        return true;
      };
    };

   ```
   ## router.js
   撰写路由，数据在controller这里来，所有的路由都在这个位置，可以新建js导入
       - 请求的名称，2：中间件，下面这个是判断有无token，3：controller文件夹下news.js内的list方法
       get请求：router.get('/news/list', app.middleware.checktoken(), controller.news.list);
       post请求：router.post()
       包含增删改查的请求router.resources('/testCrud',app.middleware.checktoken(), controller.testCrud)；controller需要：index：查看，create：添加；destroy：删除，update：更新
    例子：
 ````js
    module.exports = app => {
        const { router, controller, io } = app;
        router.get('/',controller.home.index);
        router.get('/product_active/:id',controller.home.index);
        router.resources('/banner', controller.banner)
        // socket, 指向app/io/controller/chat.js的index方法
        io.of('/').route('chat', app.io.controllers.chat.index);
    }
```` 
## controller 
自我理解：是一个获取 （servic对数据库操作之后返回的信息） 之后返回给router，数据的控制器

框架提供了一个 Controller 基类，并推荐所有的 Controller 都继承于该基类实现。这个 Controller 基类有下列属性：
ctx - 当前请求的 Context 实例。
app - 应用的 Application 实例。
config - 应用的配置。
service - 应用所有的 service。
logger - 为当前 controller 封装的 logger 对象。
在 Controller 文件中，可以通过两种方式来引用 Controller 基类：
```js
// app/controller/user.js

// 从 egg 上获取（推荐）
const Controller = require('egg').Controller;
// UserController这个方法继承了Controller
class UserController extends Controller {
  // 查询，首页展示
  async index() {
   let {ctx,app} = this
    let data = ctx.request.body // 拿到post请求数据
    let data1 = ctx.params //params  /a/a/a
    let data2 = ctx.request.query //query&
    // 获取数据库
    const result = await ctx.service.// server下的js文件名//js内的方法 // 是个函数（）
    ctx.body =  {result,data,data1,data2}
  }
}
module.exports = UserController;
```
配置路由映射：
```js
// app/router.js
module.exports = app => {
  const { router, controller } = app;
  router.get('/', controller.home.index);
};
```

## Service
```js
const Service = require('egg').Service;
class NewsService extends Service {
  /* 这里进行了数据库操作 - 已对接数据库 */
  async list() {
    let { app,ctx } = this;
    // 这个是egg内置的方法
    const res1 = await app.mysql.insert('user', { username: 'egg', phone: '123456789', create_time: '2021-08-16' });
    // 数据库原生的方法
    const res = await app.mysql.query('SELECT * FROM `user` LIMIT 0, 2');
    return res;
  }
}
module.exports = NewsService;

```
### 操作数据库
使用插件：sequelized（推荐）
```js
const Service = require('egg').Service;
class UserService extends Service {
  async findAll(where) {
    let { app } = this
    /***
    * where: { status: 'draft', author: ['author1', 'author2'] }, // WHERE 条件
    * columns: ['author', 'title'], // 要查询的表字段
    * order: [['created_at','desc'], ['id','desc']], // 排序方式
    * limit: 10, // 返回数据量
    * offset: 0, // 数据偏移量
    * 前台分页组件有时会需要查询全部记录数，这时可以直接使用count
    * where: {
    *    status: 1
    * }
     * 
     */
    let data = await app.model.Business.findAll({ limit: where.limit*1, offset: (where.page - 1) * where.limit,order:[['display_time','desc']]})
     // 查询 findAll
     // 创建：create
     // 删除 destroy
     // 更新 update
    let dataList = await app.model.Business.findAll()
    let total = dataList.length
    return { data, total: Math.ceil(total) }
  }
}
```
每一次用户请求，框架都会实例化对应的 Service 实例，由于它继承于 egg.Service，故拥有下列属性方便我们进行开发：
this.ctx: 当前请求的上下文 Context 对象的实例，通过它我们可以拿到框架封装好的处理当前请求的各种便捷属性和方法。
this.app: 当前应用 Application 对象的实例，通过它我们可以拿到框架提供的全局对象和方法。
this.service：应用定义的 Service，通过它我们可以访问到其他业务层，等价于 this.ctx.service 。
this.config：应用运行时的配置项。
this.logger：logger 对象，上面有四个方法（debug，info，warn，error），分别代表打印四个不同级别的日志，使用方法和效果与 context logger 中介绍的一样，但是通过这个 logger 对象记录的日志，在日志前面会加上打印该日志的文件路径，以便快速定位日志打印位置。

## Middleware
```js
// 中间件是一个函数
// 使用的时候在router 通过app.middleware.checktoken
function checktoken(){
  // 返回的本身有两个参数
  return async function(ctx,next){
    try{
      console.log('我是中间件的内容')
      // 获取token
      let token = ctx.request.header.token
      // 校验token
      let ddecode = ctx.app.jwt.verify(token, ctx.app.config.jwt.secret)
      //ddecode.name和数据库进项校验也可以
      if (ddecode.name) {
        await next()
      } else {
        ctx.body = '中间件返回用户不存在'
      }
      console.log(ddecode)
    }catch(e){
      ctx.body = '中间件返回用户不存在 - catch'
    } 
  }
}
module.exports = checktoken
```
router使用
```js
// 下面的请求使用了中间件  参数位：路由路径，中间件，控制器返回的内容
  router.get('/jwt', app.middleware.checktoken() ,controller.jwt.index)// jwt 生成token
  router.get('/jwtmessage', app.middleware.checktoken(), controller.jwt.getmessage)
```

## model 创建数据库模型 不完善
基本操作：
```js
module.exports = app => {
  const { STRING } = app.Sequelize
  // 默认情况下Sequelize将自动传递所有的模型名称（define的第一个参数）转换为复数
  // 这里会自动生成一个ID,数据库中有一个字段的名字叫做name
  const Banner = app.model.define('banner', {
    name: STRING,
    dscription: STRING,
    img_url: STRING,
    urllink: STRING,
  });
  return Banner
}
```

## 关联数据库表：
### 使用egg-sequelize 和 mysql2
### 一对多的关联数据
```js

module.exports = (app) => {
  const { STRING, INTEGER,DATE} = app.Sequelize;

  // 创建数据库
  const Classroom = app.model.define('class', { 
    id: { type: INTEGER, primaryKey: true, autoIncrement: true },
    name: STRING(30),
    create_by: STRING(100),
    create_time: DATE,
    update_by: STRING(100),
    update_time: DATE
  });
  
  // 声明关系
  // 一对一多使用
  Classroom.associate = function (){
    //   Classroom这个对应Student   一个班有很多学生 之后在学生表中
    /*
    *  // 声明关系
    *   Student.associate = function (){
  	*   //	学生属于一个班级，外键关联是班级id
    *   Student.belongsTo(app.model.Classroom, { foreignKey: "class_id"});
    *   }
    * */
  	// 一个班有很多学生，所以用 hasMany
    Classroom.hasMany(app.model.Student, {as: "students"});
  }
  return Classroom ;
};

```

### 多对多的关联数据
```js
// 定义模型 User
const User = sequelize.define('User', {
  // 用户属性
});

// 定义模型 Group
const Group = sequelize.define('Group', {
  // 组属性
});

// 定义中间表 UserGroup，用于连接 User 和 Group
const UserGroup = sequelize.define('UserGroup', {
  // 可选的中间表属性
});

// 建立多对多关联关系
User.belongsToMany(Group, { through: UserGroup });
Group.belongsToMany(User, { through: UserGroup });
```
在上面的示例中，我们定义了三个模型：User、Group 和 UserGroup。UserGroup 是一个中间表，用于连接 User 和 Group。通过 belongsToMany 方法，我们定义了 User 和 Group 之间的多对多关联关系，并指定了中间表 UserGroup。
通过这样的关联定义，Sequelize 将会自动为你生成适当的 SQL 查询，以处理多对多关联的数据操作。你可以通过调用生成的关联方法来访问关联的数据。
-- --
#### 不太理解的地方
```js
// 创建一个用户
const user = await User.create({ /* 用户属性 */ });

// 创建一个组
const group = await Group.create({ /* 组属性 */ });

// 将用户添加到组中
await user.addGroup(group);

// 从用户中获取关联的组
const groups = await user.getGroups();

// 从组中获取关联的用户
const users = await group.getUsers();
```
通过调用生成的关联方法，如 addGroup、getGroups 和 getUsers，你可以方便地进行多对多关联数据的操作。
这只是一个简单的示例，你可以根据你的实际需求对模型和关联进行更复杂的定义。更多关于 Sequelize 多对多关联的详细信息，你可以参考 Sequelize 文档中的相关章节。



## controller 上传文件
```js
const Controller = require('egg').Controller;
const fs = require('fs');
const path = require('path');
const awaitWriteStream = require('await-stream-ready').write;// 重点在这
const sendToWormhole = require('stream-wormhole');
const dayjs = require('dayjs');

module.exports = class extends Controller {
  // 上传未指定那个文件夹
  async upload() {
    let { ctx } = this
    const stream = await this.ctx.getFileStream();
    console.log('上传的文件')
    console.log(stream)
    // 定义文件名 
    const filename = Date.now() + path.extname(stream.filename).toLocaleLowerCase();
    console.log('上传文件名')
    console.log(filename)
    // 目标文件 
    const target = path.join('app/public/upload', filename);
    console.log('目标文件')
    console.log(target)
    // 
    const writeStream = fs.createWriteStream(target);
    console.log('writeStream')
    console.log(writeStream)

    try {
      //异步把文件流 写入 
      await awaitWriteStream(stream.pipe(writeStream));
      ctx.body = {
        ...stream.fields,
        url: '/public/upload/' + filename,
      };
    } catch (err) {
      //如果出现错误，关闭管道
      await sendToWormhole(stream)
      console.log(err)
    }
  }
  // 上传指定那个文件夹
  async uploadFile() {
    let { ctx } = this
    const stream = await ctx.getFileStream();
    console.log('上传的文件')
    console.log(stream)
    // 基础的目录 
    const uplaodBasePath = 'app/public/upload';
    console.log('基础的目录')
    console.log(uplaodBasePath)
    // 生成文件名 
    const filename = `${Date.now()}${Number.parseInt(Math.random() * 1000,)}${path.extname(stream.filename).toLocaleLowerCase()}`;
    
    // 生成文件夹 
    const dirname = dayjs(Date.now()).format('YYYY/MM/DD');
    function mkdirsSync(dirname) {
      if (fs.existsSync(dirname)) {
        return true;
      } else {
        if (mkdirsSync(path.dirname(dirname))) {
          fs.mkdirSync(dirname); return true;
        }
      }
    }
    mkdirsSync(path.join(uplaodBasePath, dirname));
    // 生成写入路径
    const target = path.join(uplaodBasePath, dirname, filename);
    // 写入流
    const writeStream = fs.createWriteStream(target);
    try {
      //异步把文件流 写入
      await awaitWriteStream(stream.pipe(writeStream));
    } catch (err) {
      //如果出现错误，关闭管道
      await sendToWormhole(stream);
    }
    
    ctx.body = { stream,url:'/public/upload/'+dirname+'/'+filename }
  }
  
}

```


# 插件
## 使用egg-sequelize 和 mysql2
1： 首先安装：npm i --save egg-sequelize msyql2
egg-sequelize
  - 1: STRING => varchar(255)
  - 2: INTEGER => int
  - 3: DOUBLE => double
  - 4: DATE => datetime
  - 5: TEXT => text
2： 在plugin.js文件中引入插件
```js
  // sequelize插件，实现数据持久化
  sequelize:{
    enable: true,
     package: 'egg-sequelize',
  },
  // 引入mysql数据库2
  mysql2: {
    enable: true,
    package: 'mysql2',
  },
```
3： 在config.default.js文件中配置这个数据库链接
```js
  // sequelized的配置文件
  config.sequelize = {
    // 数据库类型 是mysql
    dialect:"mysql",
    // host
    host: 'localhost',
    // 端口号
    port: '3306',
    // 用户名
    username: 'exapp_admin',
    // 密码
    password: '123456789',
    // 数据库名
    database: 'exapp_admin',
    // 时区
    timezone:'+08:00'
  }
```
4： 在app/model文件中创建数据模型
```js
module.exports = app => {
  const { STRING } = app.Sequelize
  // 默认情况下Sequelize将自动传递所有的模型名称（define的第一个参数）转换为复数
  // 这里会自动生成一个ID,数据库中有一个字段的名字叫做name
  const Banner = app.model.define('banner', {
    name: STRING,
    dscription: STRING,
    img_url: STRING,
    urllink: STRING,
  });
  return Banner
}

```
5： 添加app.js文件，初始化数据库
```js
  app.beforeStart(async function(){
    console.log('系统启动的时候执行 会执行这个函数 之后直接创建的数据库的表')
    //  await app.model.sync({force:true})  //每次启动删除全部的表，按照模型创建新的表 开发环境使用，会删除数据表
    // sync方法会根据模型创建一个表
    await app.model.sync({})
  })
```
6：在service中的增删改查
  // 添加
```js
   /***
    * where: { status: 'draft', author: ['author1', 'author2'] }, // WHERE 条件
    * columns: ['author', 'title'], // 要查询的表字段
    * order: [['created_at','desc'], ['id','desc']], // 排序方式
    * limit: 10, // 返回数据量
    * offset: 0, // 数据偏移量
    * 前台分页组件有时会需要查询全部记录数，这时可以直接使用count
    * where: {
    *    status: 1
    * }
     * 
     */
await app.model.Clazz.create({name:name})
// 条件添加
let data = await app.model.Clazz.findAll({ where: { id: id } })
```
  // 删除
```js
await app.model.Clazz.destroy( { where: { id: id } })
```
  // 更新
```js
    await app.model.Clazz.update({ name: name }, { where: { id: id } })
```
// 查看
```js
    let data = await app.model.Clazz.findAll()
    // 按条件查看
    let data = await app.model.Clazz.findAll({where:{id:5}})
```

原则应该在server中实现和数据库之间的交互
```js
 async findAll(){
      let { app } = this
      let data = await app.model.Clazz.findAll()
      return data
  }
```
- 添加外键
在Student通过asssociate属性指定外键
1：创建一个名为clazz的班级表，包含ID，和name两个字段
2：学生模型中添加下面的代码
```js
Student.associate = function(){ // 所属性于那本书，指向书记的主键
  app.model.Student.belongsTo(app.model.clazz,{
    foreignKey:'clazz_id',
    as:'clazz'
  })
}
```

 ##  qrcode生成二维码
 - 简单实现，在服务器端使用生成画布二维码和生成base64的二维码
 - 前端如何实现生成二维码的 需要导入此插件
 - 详细网址：https://www.npmjs.com/package/qrcode
    

## 生成token
  1：按住啊那个egg-jwt ： npm i --save egg-jwt
  2：在plugin.js引入
  ```
  jwt:{
    enable: true,
    package: 'egg-jwt',
    secret: 'siyecao'
  }
  ```
  3:config.default.js配置文件，设置secret，注意secret不能泄露
  config.jwt = {secret:'配置的加密字符串'}
  验证流程’
  1根据用户信息生成一个token 生成之后给到客户端
  2客户端将生成的token保存到浏览器中
  3每次请求的时候请求头携带token
  4服务器一年挣的时候验证token

## 上传文件 multipart
  multipart 可以接受的默认文件大小是10mb. 如果你上传一个大文件，你应该指定这个配置
```js
  config.multipart = {
    fileSize: '50mb',
    whitelist: [
      '.png', '.jpg', '.mp4','.docx'
    ],
    mode: 'stream',
  }
```

## 实时通讯egg-socket.io
```js
  // 实时通讯
  config.io = {
    // namespace命名空间配置为/
    namespace: {
      '/': {
        //  预处理器中间件, 我们这里配置了一个auth, 进行权限判断, 它对应的文件是/app/io/middleware/auth.js, 这里可以配置多个文件, 用逗号隔开
        connectionMiddleware: ['auth'], //这里我们可以做一些权限校验之类的操作// 开始 链接的时候
        packetMiddleware: ['filter'], // 通常用于对消息做预处理，又或者是对加密消息的解密等操作
      }
    },
    // 配置redis, 非必须, 不需要的可以不配置这块, egg-socket.io内置了socket-io-redis， 在cluster模式下, 使用redis可以较为简单的实现clients/rooms等信息共享
  }
```

## 模板引擎
```js
  // 配置模板引擎
  config.view = {
    // 配置视图根路径
    root: path.join(appInfo.baseDir, 'app/view'),
    // 是否缓存路径
    cache: true,
    // 配置文件默认扩展名
    defaultExtension: '.html',
    // 默认渲染模板引擎
    defaultViewEngine: 'nunjucks',
    // 文件映射配置
    mapping: {
      '.html': 'nunjucks'
    }
    
  }
```

## 跨域
```js
  // 跨站请求的伪造 不做这个会禁止 关于post请求的安全设置，// 图片上传问题
  config.security = {
    csrf: {
      enable: false,
      ignoreJSON: true,
    },
    domainWhiteList: ['http://www.hengmaikj.com'],
  }
  config.cors = {
    origin: '*', // 匹配规则  域名+端口  *则为全匹配
    allowMethods: 'GET,HEAD,PUT,POST,DELETE,PATCH',
  };
```


<a href="./egg_admin.zip">后台模板</a>
