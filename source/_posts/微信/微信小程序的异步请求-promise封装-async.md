---
uuid: 5f500955-d32f-6e41-ee8f-6c2bb8e3c16b
title: 微信小程序的异步请求-promise封装-async
date: 2020-05-16 22:23:04
tags: [微信,微信小程序的异步请求-promise封装-async]
category: 微信
---
1. 小程序原生发送异步请求: wx.request
```js
// 原生发送异步请求
wx.request({
	url: '', // 请求的路径
	method: "", // 请求的方式
	data: {}, // 请求的数据
	header: {}, // 请求头
	success: (res) => {
      	// res  响应的数据
	}
})
```
<!-- more -->
2. 用 promise 封装 wx.request
2.1 回顾promise:
```js
// 1.创建对象
const p = new Promise( (resolve,reject) => {
    // 逻辑代码
    if(){
       resolve(data)
	}else{
        reject(err)
	}
})

// 2.调用方法
p.then(res=>{
    console.log(res)
}).catch(err=>{
    console.log(err)
})
```

2.2 进行封装
```js
------------------- 第一, 在utils下新建一个js文件,进行封装 -----------------------	
// promise 特点：一创建就立即执行，一般情况下解决这个问题我们会将其封装为一个函数
// options:请求时的参数对象
function myrequest(options) {
  return new Promise((resolve, reject) => {
    // 逻辑：发送请求到服务器
    wx.request({
      url: options.url,
      method: options.method || "GET",
      data: options.data || {},
      header: options.header || {},
      success: res => {
        resolve(res);
      },
      fail: err => {
        reject(err);
      }
    });
  });
}
// 暴露给外界
export default myrequest;

----------------------- 第二, 封装完成后 引入并使用 ----------------------
import myrequest from '../../utils/api.js'
  myrequest({
    url: 'xxx',
    header: {
      'content-type': 'json' // 有些接口不需要设置
    }
  }).then(res => {
      console.log(res)
  })
  ```

3. 使用 async & await 来改造 promise
3.1 概念
​ 是 ES7 提出的新技术, 可以将 promise 对象中异步的方法以同步的方式进行书写, 减少代码量

3.2 用法
​ async：用来修饰异步代码所在的函数

​ await: 用来修改异步代码

​ 结果：异步代码会返回一个结果, 即操作完成后的结果
```js
// 使用async和await来改造上面的promise封装代码

async getList(){
    // 用res来接收这个返回值
	const res = await myrequest({
        url: 'xxx',
        header: {'content-type': 'json'} // 有些接口不需要设置
  	})
    // 直接对res进行操作
    console.log(res)
}
```

3.3 特点
​ 异步代码虽然是以同步的方式进行书写，但是依旧是异步执行的

​ await 修饰的对象一定要返回一个 promise 对象


---------
版权声明：本文为CSDN博主「陈静洁」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/EnidChann/java/article/details/100182948