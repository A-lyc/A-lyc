---
title: uniapp云开发
date: 2020-11-13 11:51:04
tags: [uniapp云开发,云开发]
category: uniapp云开发
---
* 使用HBuilder X，新建一个项目，可选择云开发基础模板，或者默认模板，新建之后会在根目录有个文件夹为：cloudfunctions，此文件夹为操作数据库文件夹看，（下图）
![新建项目](/uniapp01.png)

* 右键新建云函数，之后本地运行看是否可以读出数据，之后上传并运行，即可使用（）
举例右键创建云函数为：test
```shell
'use strict';
const db = uniCloud.database()
exports.main = async (event, context) => {
	const collection = db.collection('test')
	const res = await collection.add(event)
	return {res,name:1}
};
```
在https://unicloud.dcloud.net.cn/home上面创建服务空间 - 数据库 - 云数据库 - 添加数据库表，
之后调用：
```shell
uniCloud.callFunction({
				name:'test'
			}).then(res => {
				console.log(res)
			})
```
## cli项目中使用uniCloud
 - 如果要在cli项目中使用uniCloud，可以参考以下步骤
 - 将cli项目导入HBuilderX
 - 在项目根目录（src同级）创建cloudfunctions-aliyun或者cloudfunctions-tcb目录
 - 打开src/manifest.json，在基础配置-->uni-app应用标示处点击重新获取
 - 在步骤2创建的目录右键关联服务空间
 - 运行与发行云函数只能使用HBuilderX的菜单，不可使用package.json内的命令
 - 如果HBuilderX菜单运行不能满足需求可以考虑自行初始化服务空间服务空间初始化
 

## 获取集合引用
```shell
const db = uniCloud.database();
// 获取 `user` 集合的引用
const collection = db.collection('user');
```
## 集合 Collection 的方法
通过 db.collection(name).add() 可以获取指定集合的引用，在集合上可以进行以下操作
<table>
<thead>
<tr>
<th>类型</th>
<th>接口</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>写</td>
<td>add</td>
<td>新增记录（触发请求）</td>
</tr>
<tr>
<td>计数</td>
<td>count</td>
<td>获取符合条件的记录条数</td>
</tr>
<tr>
<td>读</td>
<td>get</td>
<td>获取集合中的记录，如果有使用 where 语句定义查询条件，则会返回匹配结果集 (触发请求)</td>
</tr>
<tr>
<td>引用</td>
<td>doc</td>
<td>获取对该集合中指定 id 的记录的引用</td>
</tr>
<tr>
<td>查询条件</td>
<td>where</td>
<td>通过指定条件筛选出匹配的记录，可搭配查询指令（eq, gt, in, ...）使用</td>
</tr>
<tr>
<td></td>
<td>skip</td>
<td>跳过指定数量的文档，常用于分页，传入 offset。clientDB组件有封装好的更易用的分页，<a href="/uniCloud/uni-clientdb-component">另见</a></td>
</tr>
<tr>
<td></td>
<td>orderBy</td>
<td>排序方式</td>
</tr>
<tr>
<td></td>
<td>limit</td>
<td>返回的结果集(文档数量)的限制，有默认值和上限值</td>
</tr>
<tr>
<td></td>
<td>field</td>
<td>指定需要返回的字段</td>
</tr>
</tbody>
</table>
where 类似于微信小程序
查询及更新指令用于在 where 中指定字段需满足的条件，指令可通过 db.command 对象取得。
collection对象的方法可以增和查数据，删和改不能直接操作，需要collection对象通过doc或get得到指定的记录后再调用remove或update方法进行删改。
## 记录 Record / Document
通过 db.collection(collectionName).doc(docId) 可以获取指定集合上指定 id 的记录的引用，在记录上可以进行以下操作
<table>
<thead>
<tr>
<th>接口</th>
<th>说明</th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td>写</td>
<td>set</td>
<td>覆写记录</td>
</tr>
<tr>
<td></td>
<td>update</td>
<td>局部更新记录(触发请求)</td>
</tr>
<tr>
<td></td>
<td>remove</td>
<td>删除记录(触发请求)</td>
</tr>
<tr>
<td>读</td>
<td>get</td>
<td>获取记录(触发请求)</td>
</tr>
</tbody>
</table>

## 查询筛选指令 Query Command
以下指令挂载在 db.command 下
<table>
<thead>
<tr>
<th>类型</th>
<th>接口</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>比较运算</td>
<td>eq</td>
<td>字段等于 ==</td>
</tr>
<tr>
<td></td>
<td>neq</td>
<td>字段不等于 !=</td>
</tr>
<tr>
<td></td>
<td>gt</td>
<td>字段大于 &gt;</td>
</tr>
<tr>
<td></td>
<td>gte</td>
<td>字段大于等于 &gt;=</td>
</tr>
<tr>
<td></td>
<td>lt</td>
<td>字段小于 &lt;</td>
</tr>
<tr>
<td></td>
<td>lte</td>
<td>字段小于等于 &lt;=</td>
</tr>
<tr>
<td></td>
<td>in</td>
<td>字段值在数组里</td>
</tr>
<tr>
<td></td>
<td>nin</td>
<td>字段值不在数组里</td>
</tr>
<tr>
<td>逻辑运算</td>
<td>and</td>
<td>表示需同时满足指定的所有条件</td>
</tr>
<tr>
<td></td>
<td>or</td>
<td>表示需同时满足指定条件中的至少一个</td>
</tr>
</tbody>
</table>

## 字段更新指令 Update Command
以下指令挂载在 db.command 下
<table>
<thead>
<tr>
<th>类型</th>
<th>接口</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>字段</td>
<td>set</td>
<td>设置字段值</td>
</tr>
<tr>
<td></td>
<td>remove</td>
<td>删除字段</td>
</tr>
<tr>
<td></td>
<td>inc</td>
<td>加一个数值，原子自增</td>
</tr>
<tr>
<td></td>
<td>mul</td>
<td>乘一个数值，原子自乘</td>
</tr>
<tr>
<td></td>
<td>push</td>
<td>数组类型字段追加尾元素，支持数组</td>
</tr>
<tr>
<td></td>
<td>pop</td>
<td>数组类型字段删除尾元素，支持数组</td>
</tr>
<tr>
<td></td>
<td>shift</td>
<td>数组类型字段删除头元素，支持数组</td>
</tr>
<tr>
<td></td>
<td>unshift</td>
<td>数组类型字段追加头元素，支持数组</td>
</tr>
</tbody>
</table>

## 时间 Date
```shell
//服务端当前时间
  new db.serverDate()

  //服务端当前时间加1S
  new db.serverDate({
    offset: 1000
  })
```
如果需要对日期进行比较操作，可以使用聚合操作符将日期进行转化，比如以下示例查询所有time字段在2020-02-02以后的记录

## 新增文档
 collection.add(data)
<table>
<thead>
<tr>
<th>参数</th>
<th>类型</th>
<th>必填</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>data</td>
<td>object | array</td>
<td>是</td>
<td>{_id: '10001', 'name': 'Ben'} _id 非必填</td>
</tr>
</tbody>
</table>

- 单条插入时
<table>
<thead>
<tr>
<th>参数</th>
<th>类型</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>id</td>
<td>String</td>
<td>插入记录的id</td>
</tr>
</tbody>
</table>

- 批量插入时
<table>
<thead>
<tr>
<th>参数</th>
<th>类型</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>inserted</td>
<td>Number</td>
<td>插入成功条数</td>
</tr>
<tr>
<td>ids</td>
<td>Array</td>
<td>批量插入所有记录的id</td>
</tr>
</tbody>
</table>

```shell
// 单条插入数据
let res = await collection.add({
  name: 'Ben'
})
// 批量插入数据
let res = await collection.add([{
  name: 'Alex'
},{
  name: 'Ben'
},{
  name: 'John'
}])
```

- 云服务商为阿里云时，若集合不存在，调用add方法会自动创建集合
方法2： collection.doc().set(data)
也可通过 set 方法新增一个文档，需先取得文档引用再调用 set 方法。 如果文档不存在，set 方法会创建一个新文档。
参数说明
<table>
<thead>
<tr>
<th>参数</th>
<th>类型</th>
<th>必填</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>data</td>
<td>object</td>
<td>是</td>
<td>更新字段的Object，{'name': 'Ben'} _id 非必填</td>
</tr>
</tbody>
</table>

响应参数
<table>
<thead>
<tr>
<th>参数</th>
<th>类型</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>updated</td>
<td>Number</td>
<td>更新成功条数，数据更新前后没变化时也会返回1</td>
</tr>
<tr>
<td>upsertedId</td>
<td>String</td>
<td>创建的文档id</td>
</tr>
</tbody>
</table>

## 查询文档
支持 where()、limit()、skip()、orderBy()、get()、field()、count() 等操作。
只有当调用get()时才会真正发送查询请求。
注：默认取前100条数据，最大取前100条数据。

### 添加查询条件
collection.where()
设置过滤条件，where 可接收对象作为参数，表示筛选出拥有和传入对象相同的 key-value 的文档。比如筛选出所有类型为计算机的、内存为 8g 的商品：
```shell
const dbCmd = db.command
let res = await db.collection('goods').where({
  category: 'computer',
  type: {
    memory: 8,
    // memory: dbCmd.gt(8), // 表示大于 8
  }
}).get()
```
### 获取查询数量
collection.count()
阿里云不同
```shell
let res = await db.collection('goods').where({
  category: 'computer',
  type: {
    memory: 8,
  }
}).count()
```
### 设置记录数量
collection.limit()
<table>
<thead>
<tr>
<th>参数</th>
<th>类型</th>
<th>必填</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>value</td>
<td>Number</td>
<td>是</td>
<td>返回的数据条数</td>
</tr>
</tbody>
</table>

```shell 
let res = await collection.limit(1).get() // 只返回第一条记录
```
### 设置起始位置
collection.skip(value)
<table>
<thead>
<tr>
<th>参数</th>
<th>类型</th>
<th>必填</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>value</td>
<td>Number</td>
<td>是</td>
<td>跳过指定的位置，从位置之后返回数据</td>
</tr>
</tbody>
</table>

```shell
let res = await collection.skip(4).get()
```
### 对结果排序
collection.orderBy(field, orderType)
<table>
<thead>
<tr>
<th>参数</th>
<th>类型</th>
<th>必填</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>field</td>
<td>string</td>
<td>是</td>
<td>排序的字段</td>
</tr>
<tr>
<td>orderType</td>
<td>string</td>
<td>是</td>
<td>排序的顺序，升序(asc) 或 降序(desc)</td>
</tr>
</tbody>
</table>

如果需要对嵌套字段排序，需要用 "点表示法" 连接嵌套字段，比如 style.color 表示字段 style 里的嵌套字段 color。
同时也支持按多个字段排序，多次调用 orderBy 即可，多字段排序时的顺序会按照 orderBy 调用顺序先后对多个字段排序
使用示例
```shell
let res = await collection.orderBy("name", "asc").get()
```
### 指定返回字段
collection.field()
从查询结果中，过滤掉不需要的字段，或者指定要返回的字段。
<table>
<thead>
<tr>
<th>参数</th>
<th>类型</th>
<th>必填</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>-</td>
<td>object</td>
<td>是</td>
<td>过滤字段对象，包含字段名和策略，不返回传false，返回传true</td>
</tr>
</tbody>
</table>

```shell 
collection.field({ 'age': true }) //只返回age字段，其他字段不返回
```
备注：只能指定要返回的字段或者不要返回的字段。即{'a': true, 'b': false}是一种错误的参数格式
### 查询指令
查询指令以dbCmd.开头，包括等于、不等于、大于、大于等于、小于、小于等于、in、nin、and、or。

下面的查询指令以以下数据集为例：

## 删除文档
### 方式1 通过指定文档ID删除
collection.doc(_id).remove()
```shell
// 清理全部数据
let res = await collection.get()
res.data.map(async(document) => {
  return await collection.doc(document.id).remove();
});
```
### 方式2 条件查找文档然后直接批量删除
collection.where().remove()
```shell
// 删除字段a的值大于2的文档
const dbCmd = db.command
let res = await collection.where({
  a: dbCmd.gt(2)
}).remove()

// 清理全部数据
const dbCmd = db.command
let res = await collection.where({
  _id: dbCmd.exists(true)
}).remove()
```
例子：
```shell
const db = uniCloud.database();
db.collection("table1").doc("5f79fdb337d16d0001899566").remove()
    .then((res) => {
        console.log("删除成功，删除条数为: ",res.deleted);
    })
    .catch((err) => {
        console.log( err.message )
    })
    .finally(() => {

    })
```

## 更新文档
### 更新指定文档
collection.doc().update(Object data)
update参数
<table>
<thead>
<tr>
<th>参数</th>
<th>类型</th>
<th>必填</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>data</td>
<td>object</td>
<td>是</td>
<td>更新字段的Object，{'name': 'Ben'} _id 非必填</td>
</tr>
</tbody>
</table>

响应参数
<table>
<thead>
<tr>
<th>参数</th>
<th>类型</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>updated</td>
<td>Number</td>
<td>更新成功条数，数据更新前后没变化时会返回0</td>
</tr>
</tbody>
</table>

```shell
// 更新 文档
let res = await collection.doc('doc-id').update({
  name: "Hey",
  count: {
    fav: 1
  }
});

// 更新前
{
  "_id": "doc-id",
  "name": "Hello",
  "count": {
    "fav": 0,
    "follow": 0
  }
}

// 更新后
{
  "_id": "doc-id",
  "name": "Hey",
  "count": {
    "fav": 1,
    "follow": 0
  }
}
```

更新数组时，已数组下标作为key即可，比如以下示例将数组arr内下标为1的值修改为 uniCloud
```shell
 // 更新数组时
let res = await collection.doc('doc-id').update({
  arr: {
    1: "uniCloud"
  }
})
// 更新前
{
  "_id": "doc-id",
  "arr": ["hello", "world"]
}
// 更新后
{
  "_id": "doc-id",
  "arr": ["hello", "uniCloud"]
}
```

### 更新文档，如果不存在则创建
collection.doc().set()
此方法会覆写已有字段，需注意与update表现不同，比如以下示例执行set之后follow字段会被删除

```shell
let res = await collection.doc('doc-id').set({
  name: "Hey",
  count: {
    fav: 1
  }
})
// 更新前
{
  "_id": "doc-id",
  "name": "Hello",
  "count": {
    "fav": 0,
    "follow": 0
  }
}

// 更新后
{
  "_id": "doc-id",
  "name": "Hey",
  "count": {
    "fav": 1
  }
}
```

### 批量更新文档
collection.update()
```shell
const dbCmd = db.command
let res = await collection.where({name: dbCmd.eq('hey')}).update({
  age: 18,
})
```

### 更新数组内指定下标的元素
```shell
const res = await db.collection('query').doc('1').update({
  // 更新students[1]
  ['students.' + 1]: {
    name: 'wang'
  }
})
// 更新前
{
  "_id": "1",
  "students": [
    {
      "name": "zhang"
    },
    {
      "name": "li"
    }
  ]
}

// 更新后
{
  "_id": "1",
  "students": [
    {
      "name": "zhang"
    },
    {
      "name": "wang"
    }
  ]
}

```

### 更新数组内匹配条件的元素
注意：只可确定数组内只会被匹配到一个的时候使用

```shell
const res = await db.collection('query').where({
    'students.id': '001'
}).update({
  // 将students内id为001的name改为li
    'students.$.name': 'li'
})
// 更新前
{
  "_id": "1",
  "students": [
    {
      "id": "001",
      "name": "zhang"
    },
    {
      "id": "002",
      "name": "wang"
    }
  ]
}

// 更新后
{
  "_id": "1",
  "students": [
    {
      "id": "001",
      "name": "li"
    },
    {
      "id": "002",
      "name": "wang"
    }
  ]
}
```

### 更新操作符
https://uniapp.dcloud.io/uniCloud/cf-database?id=%e6%9b%b4%e6%96%b0%e6%93%8d%e4%bd%9c%e7%ac%a6

## GEO地理位置
注意：如果需要对类型为地理位置的字段进行搜索，一定要建立地理位置索引。
### GEO数据类型
#### Point
用于表示地理位置点，用经纬度唯一标记一个点，这是一个特殊的数据存储类型。

签名：Point(longitude: number, latitude: number)

示例：

```shell
new db.Geo.Point(longitude, latitude)
```
#### LineString
用于表示地理路径，是由两个或者更多的 Point 组成的线段。
签名：LineString(points: Point[])
示例：
```shell
new db.Geo.LineString([
  new db.Geo.Point(lngA, latA),
  new db.Geo.Point(lngB, latB),
  // ...
])
```
#### Polygon
用于表示地理上的一个多边形（有洞或无洞均可），它是由一个或多个闭环 LineString 组成的几何图形。

由一个环组成的 Polygon 是没有洞的多边形，由多个环组成的是有洞的多边形。对由多个环（LineString）组成的多边形（Polygon），第一个环是外环，所有其他环是内环（洞）。

签名：Polygon(lines: LineString[])

示例：
```shell
new db.Geo.Polygon([
  new db.Geo.LineString(...),
  new db.Geo.LineString(...),
  // ...
])
```
#### MultiPoint
用于表示多个点 Point 的集合。

签名：MultiPoint(points: Point[])

示例：
```shell
new db.Geo.MultiPoint([
  new db.Geo.Point(lngA, latA),
  new db.Geo.Point(lngB, latB),
  // ...
])

```
#### MultiLineString
用于表示多个地理路径 LineString 的集合。

签名：MultiLineString(lines: LineString[])

示例：
```shell
new db.Geo.MultiLineString([
  new db.Geo.LineString(...),
  new db.Geo.LineString(...),
  // ...
])

```
#### MultiPolygon
用于表示多个地理多边形 Polygon 的集合。

签名：MultiPolygon(polygons: Polygon[])

示例：
```shell
new db.Geo.MultiPolygon([
  new db.Geo.Polygon(...),
  new db.Geo.Polygon(...),
  // ...
])
```

### GEO操作符
#### geoNear
按从近到远的顺序，找出字段值在给定点的附近的记录。

签名：
```shell
db.command.geoNear(options: IOptions)

interface IOptions {
  geometry: Point // 点的地理位置
  maxDistance?: number // 选填，最大距离，米为单位
  minDistance?: number // 选填，最小距离，米为单位
}
```
示例：
```shell
let res = await db.collection('user').where({
  location: db.command.geoNear({
    geometry: new db.Geo.Point(lngA, latA),
    maxDistance: 1000,
    minDistance: 0
  })
}).get()

```
#### geoWithin

找出字段值在指定 Polygon / MultiPolygon 内的记录，无排序

签名：
```shell
db.command.geoWithin(IOptions)

interface IOptions {
  geometry: Polygon | MultiPolygon // 地理位置
}

```
示例：
```shell
// 一个闭合的区域
const area = new Polygon([
  new LineString([
    new Point(lngA, latA),
    new Point(lngB, latB),
    new Point(lngC, latC),
    new Point(lngA, latA)
  ]),
])

// 搜索 location 字段在这个区域中的 user
let res = await db.collection('user').where({
  location: db.command.geoWithin({
    geometry: area
  })
}).get()

```
#### geoIntersects
找出字段值和给定的地理位置图形相交的记录

签名：
```shell
db.command.geoIntersects(IOptions)

interface IOptions {
  geometry: Point | LineString | MultiPoint | MultiLineString | Polygon | MultiPolygon // 地理位置
}

```
示例：
```shell
// 一条路径
const line = new LineString([
  new Point(lngA, latA),
  new Point(lngB, latB)
])

// 搜索 location 与这条路径相交的 user
let res = await db.collection('user').where({
  location: db.command.geoIntersects({
    geometry: line
  })
}).get()
```

## 事务
