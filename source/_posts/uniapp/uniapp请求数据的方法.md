---
uuid: 1e77cabd-9e5c-26c0-9b03-175ff22eb9f2
title: uniapp请求数据的方法
date: 2020-04-11 19:08:26
tags: uniapp
category: uniapp
---
<!-- more -->
```
__init(){
uni.request(
				{
					//第一种
					url:"http://ceshi3.dishait.cn/api/index_category/data",
					method:'GET',
					data:{},
					success: (res) => {
						console.log(res)
					},
					fail: () => {
						console.log("请求失败")
					},
					complete: () => {
						console.log("请求完成")
					}
				}
				)


				////第二种prominse
				uni.request({
					url:"http://ceshi3.dishait.cn/api/index_category/data",
					method:"GET"
				}).then(data => {
					
					let [reeor,result] = data
					console.log(reeor)//错误的时候
					console.log(result)//正确的时候
					
					if(reeor){
						return console.log("错误")
					}
					
					if(result.statusCode !== 200){
						return console.log("请求错误")
					}
					console.log(result.data)
				})
				
				//第三种需要在函数最前面加async 是一个异步便同步的配合await这个使用
				let [error,result] =  await uni.request({
					url:"http://ceshi3.dishait.cn/api/index_category/data",
					method:"GET"
				})
					if(error){
						return console.log("错误")
					}
					
					if(result.statusCode !== 200){
						return console.log("请求错误")
					}
					let data = result.data.data

					console.log(data)
					//初始化tab
					this.tabBars = data.category
					
					//初始化页面
					this.newsitems = this.setNewsitems(data)
          }
```