---
uuid: 58664c45-bcd5-e8d1-ab2d-d66ce0118816
title: ES6的简单语法
date: 2020-03-25 14:59:58
tags: ES6
category: ES6
---

> ES6 简单的几个语法 const：常量   let：变量  具有闭包局部作用域
> 找到data对象中，中括号内的元素，let { small_banner, top_banner, activity, news, product } = data
> () => {}箭头函数相当于function(){}
> ...res   循环遍历res
> 对象语法：let name = { name：name} 可修改成 let name = {name}
> export default  到出 
> import {} from ''导入

> :before  在父元素上面   :after   在父级元素底下

>处理异步操作的时候使用的
  ```
  new Promise((resolve,reject)=>{}).then(()=>{成功}).catch(()=>{失败})
  ```
固定写法

> ES6为Array增加了find()，findIndex函数。
* find()函数用来查找目标元素，找到就返回该元素，找不到返回undefined或者找不到就返回-1，找到第一个元素返回。
  findIndex()函数也是查找目标元素，找到就返回元素的位置，找不到就返回-1，找到符合条件数组的位置。
  他们的都是一个查找回调函数。

```
[1, 2, 3, 4].find((value, index, arr) => {
})
```

* 查找函数有三个参数。
  value：每一次迭代查找的数组元素。
  index：每一次迭代查找的数组元素索引。
  arr：被查找的数组。

* for(let i in this.arr){
    this.arr[i]
  }
  for(let book of this.books){
    books.XXX
  }

### 我们可以使用 JSON.parse() 方法将数据转换为 JavaScript 对象  接收的JSON转成JavaScript

### 我们可以使用 JSON.stringify() 方法将 JavaScript 对象转换为字符串。 JavaScript转成JSON格式传给服务器