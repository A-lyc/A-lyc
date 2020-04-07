---
uuid: adafc504-4b9e-6f82-dc48-6cbf585afabf
title: uniapp监听事件
date: 2020-04-07 19:23:04
tags: uniapp
category: uniapp
---

###  uniapp监听事件
+ 原生导航搜索按钮点击事件,可以和data同级别
```
onNavigationBarButtonTap() {
			uni.navigateTo({
				url:'../SearchList/SearchList'
			})
		}
```
+ 点击搜索框事件，可以和data同级别
```
uni.onNavigationBarSearchInputClicked(() =>{
				uni.navigateTo({
					url:'../search/search'
				})
				console.log(123)
			})
```