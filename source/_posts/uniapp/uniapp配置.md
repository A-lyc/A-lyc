---
title: uniapp H5适配问题
date: 2022-10-09 11:51:04
tags: [manifest.json,uniapp,配置]
category: uniapp云开发
---

## 关闭一打开页面的一个提示问题
```json
"compatible" : {
    "ignoreVersion" : true //true表示忽略版本检查提示框，HBuilderX1.9.0及以上版本支持  
},
```
## H5打包会顶部有一部分 去除顶部那一部分的
```js
var wv;//计划创建的webview
export default {
	onLoad() {
		setTimeout(()=>{
			this.operation()
		},500)
	},
	onBackPress(e) {
		// 在这里直接默认所有的返回键操作都去执行h5的方法
		this.webView.evalJS('window.myHistory()')
		return true; // 返回true 表示不执行返回键默认操作
	},
	onReady() {
		// #ifdef APP-PLUS
		let height = 0; //定义动态的高度变量
		let statusbar = 0; // 动态状态栏高度
		uni.getSystemInfo({ // 获取当前设备的具体信息
			success: (sysinfo) => {
				statusbar = sysinfo.statusBarHeight;
				height = sysinfo.windowHeight;
			}
		});
		var currentWebview = this.$scope.$getAppWebview() //此对象相当于html5plus里的plus.webview.currentWebview()。在uni-app里vue页面直接使用plus.webview.currentWebview()无效
		setTimeout(function() {
			wv = currentWebview.children()[0]
			wv.setStyle({top:statusbar,height: height - statusbar,})
		}, 1000); //如果是页面初始化调用时，需要延时一下
		// #endif
	},
	methods: {
		operation() {
			// #ifdef APP-PLUS
			// 此对象相当于html5plus里的plus.webview.webView()。
			// 在uni-app里vue页面直接使用plus.webview.webView()无效，
			// 非v3编译模式使用this.$mp.page.$getAppWebview()
			this.webView = this.$mp.page.$getAppWebview().children()[0]
			//如果是页面初始化调用时，需要 setTimeout 延时一下
			// #endif
		},
		getH5Message(e) {
			var item = e.detail.data[0]
			switch (item.type) {
				case 'back':
					this.operation()
					break;
				case 'outApp':
					this.back()
					break;
				default:
					break;
			}
		},
		// 退出app
		back() {
			uni.showModal({
				title: '提示',
				content: '是否退出系统？',
				success: function(res) {
					if (res.confirm) {
						// 退出当前应用，该方法只在App中生效
						plus.runtime.quit()
					} else if (res.cancel) {
						console.log('用户点击取消')
					}
				}
			})
		},
	},
};

```