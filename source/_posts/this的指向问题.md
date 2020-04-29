---
uuid: 4d8b8aad-1336-325b-d319-326a2c73b1fc
title: this的指向问题
date: 2020-04-29 23:02:11
tags: ES6
category: ES6
---
this.的指向问题，this在函数内是指向的win，无论嵌套多少个函数this的指向永远是win，

this指向的是一个对象，this最终指向调用它的对象。谁调用她，他就指向谁
```js
let obj = {
  name:"lyc",
  a:{
    name:"123",
    fn:function(){
    console.log(this)//指向的是a  {name:123,fn:f()}
  }
  }
  fn:function(){
    console.log(this)//指向的是obj
  }
}

let obj2 = obj.fn
obj2()//指向的是win

let obj2 = obj.fn()//指向obj
```
个人认为：
```js
let a = new obj(){
  name:"lyc",
  fn:function(){
    console.log(this)//指向的是a  {name:"lyc",fn:f(),a{...}}
  }
  a:{
    then = this
    name:"123",
    fn:function(){
    console.log(this)//指向的是a  {name:123,fn:f()}
  }
  }
}
```
a就是一个对象，因为实例化对象，哈哈哈


例子：
```js
    let obj = {
      name: '01最外层的this',
      set: 18,
      fon: function () {
        console.log(this) //this的指向是---{name: "01最外层的this", set: 18, fon: ƒ}
      },
      fn: {
        name: '02外层中的fn内的this',
        fon: function () {
          console.log(this) // this的指向是---{name: "02外层中的fn内的this", fon: ƒ}
          let arr = [1,2,3] // 定义数组
          let then = this // 重定义this的指向
          let obj = {
            name: '04函数内的this',
            set: 18,
            fon: function () {
              console.log(this) // this的指向是---{name: "03函数内的this", set: 18, fon: ƒ}
              console.log(arr)  // 函数内的数组 ---[1, 2, 3]
              console.log(then) // this的指向是---{name: "02外层中的fn内的this", fon: ƒ}
            },
          }
          console.log(obj.fon())
        },
      }
    }
    obj.fn.fon() //
```