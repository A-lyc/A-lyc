---
uuid: 4cab8886-8c35-e9c9-49a7-51c64bcb2e41
title: uniapp数据请求封装应用
date: 2020-04-11 21:45:03
tags: uniapp
category: uniapp
---


+ uniapp数据请求封装，存到公共文件夹内，之后引入,内有介绍，挺详细的

```
export default {
	// 全局配置
	common:{
    //请求网址
		baseUrl:"http://www.wangzhi.cn/api",
		//header头
    header:{
			'Content-Type':'application/json;charset=UTF-8',
			'Content-Type':'application/x-www-form-urlencoded'
		},
    //数据
		data:{},
    //请求方式
		method:'GET',
    //请求格式
		dataType:'json'
	},
	// 请求 返回promise
	request(options = {}){
		// 组织参数
		options.url = this.common.baseUrl + options.url
		options.header = options.header || this.common.header
		options.data = options.data || this.common.data
		options.method = options.method || this.common.method
		options.dataType = options.dataType || this.common.dataType
		// 请求
		return new Promise((res,rej)=>{
			// 请求之前... todo
			// 请求中...
			uni.request({
				...options,
				success: (result) => {
					// 服务端失败
					if(result.statusCode !== 200){
						uni.showToast({
							title: result.data.msg || '服务端失败',
							icon: 'none'
						});
						return rej() 
					}
					// 请求成功
					let data = result.data.data
					res(data)
				},
        // 请求失败
				fail: (error) => {
					uni.showToast({
						title: error.errMsg || '请求失败',
						icon: 'none'
					});
					return rej()
				}
			});
		})
	},
	// get请求
	get(url,data = {},options = {}){
		options.url = url
		options.data = data
		options.method = 'GET'
		return this.request(options)
	},
	// post请求
	post(url,data = {},options = {}){
		options.url = url
		options.data = data
		options.method = 'POST'
		return this.request(options)
	}
}
```

+ get请求，不传递参数

```
//定义的变量和url
$H.get('/index_category/data').then((res) =>{
					console.log("请求成功")
				}).catch(() =>{
					console.log('请求失败')
				})
```

+ 使用1:请求的时候传递参数过去post和get
```
let R = await $H.get('/index_category/data',{name:this.name,age:this.age})

//给/index_category/data传输参数，post请求传输的是name，age，传过去的参数返回是一个Promise可以使用.then接收
this.$H.post('/index_category/data',{name:this.name,age:this.age}).then(res => {})
```

+ 使用2:需要引入之后自定义一个名称，

```
$H.request({
  // 网址后面拼接的地址可拼接
	url:'/index_category/data' + this.id + '/data/' + this.iid,
	}).then((res) =>{
		console.log("请求成功")
	}).catch(() =>{
		console.log('请求失败')
	})
				
```