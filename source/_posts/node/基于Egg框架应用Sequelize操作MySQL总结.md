title: 基于Egg框架应用Sequelize操作MySQL总结
date: 2022-03-04 18:32:25
tags: Egg框架,Sequelize,MySQL总结,MySQL
category: 基于Egg框架应用Sequelize操作MySQL总结
---
Sequelize，是一个广泛使用的 ORM 框架，它支持 MySQL、PostgreSQL、SQLite 和 MSSQL 等多个数据源。
如何在 egg 项目中使用 sequelize，请查看Egg项目中使用Sequelize。
如果需要获取更多Sequelize的功能，可以查看 <a herf='https://www.sequelize.com.cn/'> Sequelize 中文文档。</a>

# 1.安装注意点
```shell script
npm install --save egg-sequelize mysql2
```
如果遇到报错：”Please install mysql2 package manually“，
解决方案：删除node_modules，使用npm安装，不要用cnpm；

# 2.配置数据库-时区配置
在config.json文件中，配置完开发环境和测试环境数据库信息后，默认时区为美国时间，需要添加如下配置，设置为北京时间，后续在创建表、更新表结构时，时间就不会出错了。
```shell script
timezone: '+08:00'
```

# 否修改表名为复数配置
默认情况下，我们在model文件内和数据库关联时，还是拿上述示例来说，如果我们需要在model文件夹创建users.js和users表进行关联，需要这么写
```shell script
'use strict';

module.exports = app => {
  const { STRING, INTEGER, DATE } = app.Sequelize;

  const Users = app.model.define('user', {
    id: { type: INTEGER, primaryKey: true, autoIncrement: true },
    name: STRING,
    created_at: DATE,
    updated_at: DATE,
  });

  return Users;
};
```
需要注意的是，app.model.define()的第一个参数是user，而不是users，这也是笔者曾经踩过的坑。为什么呢，网上了解到说，是因为squelize会将user默认转变为复数

如果不想使用默认变更为复数配置，解决方案有两种：
(1).对app.model.define的第三个参数进行配置，整体代码如下：
```shell script
'use strict';

module.exports = app => {
  const { STRING, INTEGER, DATE } = app.Sequelize;

  const Users = app.model.define('user', {
    id: { type: INTEGER, primaryKey: true, autoIncrement: true },
    name: STRING,
    created_at: DATE,
    updated_at: DATE,
  }, {
    freezeTableName: true, // 默认false修改表名为复数，true不修改表名，与数据库表名同步
  });

  return Users;
};
```
(2).在config.json文件中，添加如下配置：

```shell script
"define": {
   "freezeTableName": true
}
```
这种方式的优势是，对新增的其他表结构，都会设置不修改表名为复数。

# 表关联
我们实现具体业务，往往需要创建多张表，而表与表之间需要进行关联，如：一对一、一对多、多对多等，具体可以参考<a href="https://www.sequelize.com.cn/core-concepts/assocs">Sequelize 中文文档 - 关联。</a>      

对于关联的方式，我们可以写在service文件夹里面，也可以写在model文件夹里面。经过个人实践，service文件夹里面主要是写的业务接口，所以不推荐使用。model文件夹里面是实现表结构连接数据库，这里比较适合写表关联逻辑。
创建关联A表和B表关联 创建了未使用，需要操作使用 <a href='#guanlian'>表关联后</a>
(1).一对多关联：A 有多个 B（如一个用户下面有多个订单）
```shell script
'use strict';

module.exports = app => {
  const { STRING, INTEGER, DATE } = app.Sequelize;

  const A = app.model.define('A', {
    id: { type: INTEGER, primaryKey: true, autoIncrement: true },
    name: { type: STRING, unique: true },
    created_at: DATE,
    updated_at: DATE,
  });

  // 创建关联A表和B表关联，即：A 有多个 B
  A.associate = function() {
    app.model.A.hasMany(app.model.B, {
      foreignKey: 'B_id', // 指定外键这个外键要和A表的ID相对应，这样才能找到是哪一个下面的
    });
  };
/*
如果A表有个ID：1
那么想要关联B表有数据的话，B表中的B_id需要和A表中的ID相等
*/
  return A;
};
```

(2).一对多关联：C 属于 D（如一个订单属于所有订单中的其中之一，和上述例子逻辑相反）
```shell script
'use strict';

module.exports = app => {
  const { STRING, INTEGER, DATE } = app.Sequelize;

  const D = app.model.define('D', {
    id: { type: INTEGER, primaryKey: true, autoIncrement: true },
    name: STRING,
    created_at: DATE,
    updated_at: DATE,
  });

  // C表和D表关联，即：C 属于 D
  D.associate = function() {
    app.model.C.belongsTo(app.model.D, {
      foreignKey: 'c_id', // 指定外键
    });
  };

  return Device;
};
```


<a name='guanlian'></a>
# 表关联后，获取的数据修改key值 
 ## server文件夹
(1).按照上述例子，如果A表和B表关联后，可以通过如下方式获取：
attributes：不传全部返回
必要的进行确认关联：include:[{model:"关联的数据库表",attributes:['关联之后返回的属性']}]
```shell script
    const res = await A.findAll({
        attributes: [ 'id', 'path' ],
        include: [{
          model: B,
          attributes: [ 'id', 'url',],
        }],
        raw: false,
      });
```
得到的结果为：
```shell script
  [{
        id: 1,
        path: "https://xxx.com/test.html",
        t_urls: [{
            id: 101,
            url: "https://xxx.cdn.com/a.png",
        },
        {
            id: 102,
            url: "https://xxx.cdn.com/b.png"
        }]
    }]
```
如果想修改t_urls的key为urlList，可以手动转换：
```shell script
const targetRes = res.map(({ id, path, t_urls }) => {
    return { id, path, urlList: t_urls };
});
```
(2).按照上述例子，如果C表和D表关联后，可以通过如下方式获取：
```shell script
    const res = await C.findOne({
        attributes: [ 'url', 'modify' ],
        where: { id: urlId },
        include: [{
          model: D,
          attributes: [ 'ua', 'device' ],
        }],
        raw: false,
      });
```
获取的数据结构如下：
 ```shell script
    data: {
        url: "http://xxx.cdn.com/1617278622883.png",
        modify: "666",
        t_device: {
            ua: "chrome",
            device: "HUAWEI"
        }
    },
    code: 0,
    msg: "操作成功"
}
```
如果想修改t_device的key，把ua和device调整为和url、modify同级，可以手动转换：
```shell script
// 这里需要注意的是，对res的操作，一定要先JSON.stringify，再JSON.parse
// 因为res对象不仅包含上述值，还包含有表结构相关信息
const targetRes = JSON.parse(JSON.stringify(res));
const { ua = '', device = '' } = targetRes.t_device || {};
delete targetRes.t_device;
targetRes.ua = ua;
targetRes.device = device;
```

# 参考文档
<a href="https://eggjs.org/zh-cn/intro/index.html">Egg.js 是什么? - 为企业级框架和应用而生</a>

<a href="https://eggjs.org/zh-cn/tutorials/sequelize.html">Sequelize - 为企业级框架和应用而生</a>

<a href="https://www.sequelize.com.cn/">Sequelize 中文文档 | Sequelize 中文网</a>

<a href="https://www.sequelize.com.cn/core-concepts/assocs">关联 | Sequelize 中文网</a>

<a href="https://blog.tcs-y.com/2020/05/14/sequelize-migrations/">sequelize 数据库迁移命令 migrations</a>
