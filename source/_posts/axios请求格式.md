---
title: axios请求格式
date: 2022-06-18 13:48:15
tags: axios 请求格式 post
category: js
---
## axios 表单（formData）方式提交请求，以及文件上传方式
 ### 使用表单格式请求

更改请求头 headers 中 content-type 为 application/x-www-form-urlencoded

axios 官方文档 application/x-www-form-urlencoded 格式请求

例子：
```js
import qs from 'qs';

const data = { 'bar': 123 };
const options = {
  method: 'POST',
  headers: { 'content-type': 'application/x-www-form-urlencoded' },
  data: qs.stringify(data),
  url,
};
axios(options);
```
qs 是一个 查询字符串解析增加安全性的一个库

如果使用 vue 脚手架创建项目，会自动安装 qs 库，没有就手动引入或者安装
<a href="https://www.npmjs.com/package/qs">qs 库 npm 地址</a>

qs.stringify 做了什么，简单来讲，类似于（不仅仅如此，有兴趣了解详情可以看源码）

```js
let params = {
	name: '小明',
	age: 18
}
let qStr = Object.keys(params).map(v => `${v}=${encodeURI(params[v])}`).join('&')
```

// qStr 输出 为 name=%E5%B0%8F%E6%98%8E&age=18

这是格式化看起来很像，get 请求中的 参数
### URLSearchParams
这个写法和上面的qs是一样的 请求头需要自己加
```js
const params = new URLSearchParams()
for(let key in data){
    params.append(key,data.key)
}
// 请求
axios.post(url,params,{headers: { 'content-type': 'application/x-www-form-urlencoded' }});
```

### 上传文件
上传文件一般都会使用表单数据（formData）上传

使用 FormData 构造函数
<a href="https://developer.mozilla.org/zh-CN/docs/Web/API/FormData">
查看 FormData 构造函数 API
</a>
例子：
```js
<input id="name" name="name"/>
<input id="age" name="age"/>
<input id="file" type="file" name="file" multiple>
let forms = new FormData()
forms.append('name', document.getElementById('name').value)
forms.append('age', document.getElementById('age').value)
let files = document.getElementById('file').files
// 上传多个文件
Array.from(files).forEach(item => {
	forms.append('file', item)
})
const options = {
  method: 'POST',
  // headers: { 'content-type': 'application/x-www-form-urlencoded' },
  data: forms,
  url,
};
axios(options);
```

总结
文件上传 并不一定需要 更改请求头 headers 中 content-type 为 application/x-www-form-urlencoded

只需要使用 FormData 格式的数据即可，具体怎么传，还得根据后端接口对应更改


原文链接：https://blog.csdn.net/rudy_zhou/article/details/113252392