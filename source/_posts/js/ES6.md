---
title: ES6
date: 2020-08-19 20:14:36
tags: ES6
category: ES6
---
定义变量 let 
定义常量 const：声明一个只读的常量。一旦声明，常量的值就不能改变。
魔法字符串 '`${}`'
结构语法 let {data} = res;let [a,b,c] = arr
函数默认语法 f function f( x = {}){return x} // 默认空对象
遍历追加数组 arr.push(...array)

### class
```js
class Calc {
  constructor(){
    console.log('每次都会走这个地方，可以不设置这个函数');
  }
  add(a,b){
    console.log('这里是使用到的函数')
    return a + b
  }
}
var c = new Calc()
c.d = 10
console.log(c)// {d:10}
console.log(c.add(1,2))// 此方法在原型上 __proto__ 属性上的类
```
### function的默认值
```js 
function(x=1){
  console.log(x)// 1
}
```
### try{}catch(e){}
  捕捉try内的一个错误之后执行catch内的方法
### 引入global作为顶层对象
```js
// CommonJS的写法
var global = require('system.global')();

// ES6模块的写法
import global from 'system.global'; global();
```


### 变量结构赋值
```js
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3
```