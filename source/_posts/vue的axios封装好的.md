---
uuid: 1e93b16c-dc93-a055-b245-7494c2659ddb
title: vue的axios封装好的
date: 2020-03-28 17:17:43
tags: vue
category: vue
---

下载安装vue - axios ，npm install vue-axios --save，，通过axios和后端链接，获取数据，
```
import axios from 'axios'

//git请求
export function request(config) {
  // 1.创建axios的实例
  const instance = axios.create({
    baseURL: '网址链接的json数据',
  })

  // 2.axios的拦截器
  // 2.1.请求拦截的作用
  instance.interceptors.request.use(config => {
    console.log("请求拦截器")
    return config
  },err => {
    return err.data
  })

  // 2.2.响应拦截
  instance.interceptors.response.use(res => {
    console.log("响应拦截")
    return res.data
  },err => {
    return err.data
  })

  // 3.发送真正的网络请求
  return instance(config)
}

```

如果后端需要获取数据可传输数据给后端：params:是传输数据的参数
```
import {request} from "./request";

export function getHomeMultidata(){
  return request({
    url: 'XXX',
    method:'post',
    data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
  })
}

// get请求传值
export function getHomeGoods(type,page){
  return request({
    url: 'XXX',//`XXX>?data=${data}&page=${page}`
    //传到后端一个type和page获取相应的数据
    params:{
      type,
      page
    }
  })
}

```

应用：created生命周期内

```
 created(){
  this.getHomeMultidata().then(res => {
  console.log(res)
})
 }
```

最好这样写：
```
methods: {
  getHomeMultidata() {
    getHomeMultidata().then(res => {
      console.log(res);
    }).catch(() => {
      console.log('请求失败')
    });
  },
 },
 created(){
   this.getHomeMultidata().then(res => {
 console.log(res)
})
 }

```

### 可以封装一个class类来储存数据
```
export class Goods{
  constructor(itemInfo){//异步请求之后传来的值
    this.title = itemInfo.title//this指向当前实例对象
  }
}
```
使用：在总的数据请求中把数据给到data中的Goods
```
 this.Goods = new Goods(res.result.itemInfo}//把异步请求到的数据传输给Goods，之后Goods接收之后处理接收的信息
 // data中的this.Goods进行接收
```

常识：

导入main.js：import axios from 'axios'
使用：
导出：export class Goods{}//导出一个类名为Goods的
```
axios({
url:'',
method:''//修改请求方式（个get等）
params:{//针对get请求参数拼接
  type:"pop",
  page:1
}
}).then( res => {
console.log(res)
}).catch(()=>{
  console.log('失败')
})
```
params：{}  get参数拼接的
服务器真实返回的是data的数据
axis请求方式
常见的配置选项
请求多个数据，一起返回结果使用axios.all([axios(),axios()]).then(()=>{})///all里面是数组
全局配置axios.defaults.baseURL = "";
创建实例的axios
```
	const app = axios.create({
 公用的如：baseUrl：''
  })
	使用：
app({
url:"",
}).then(()=>{})
```

请求方式
axios(config)

axios.request(config)

axios.get(url[, config])

axios.delete(url[, config])

axios.head(url[, config])

axios.post(url[, data[, config]])

axios.put(url[, data[, config]])

axios.patch(url[, data[, config]])

常见的配置
请求地址
url: '/user',

请求类型
method: 'get',

请根路径
baseURL: 'http://www.mt.com/api',

请求前的数据处理
transformRequest:[function(data){}],

请求后的数据处理
transformResponse: [function(data){}],

自定义的请求头
headers:{'x-Requested-With':'XMLHttpRequest'},

URL查询对象
params:{ id: 12 },

查询对象序列化函数
paramsSerializer: function(params){ }
request body
data: { key: 'aa'},

超时设置s
timeout: 1000,

跨域是否带Token
withCredentials: false,

自定义请求处理
adapter: function(resolve, reject, config){},

身份验证信息
auth: { uname: '', pwd: '12'},

响应的数据格式 json / blob /document /arraybuffer / text / stream
responseType: 'json',

