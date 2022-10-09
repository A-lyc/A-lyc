---
uuid: 2e13fe43-5e09-4bdd-e396-0e01f9bce214
title: uniapp实现像浏览器一样前进后退的功能 uniapp在webview怎么后退返回
date: 2022-10-09 13:25:06
tags: uniapp webview怎么后退返回 uniapp前进后退
category: uniapp webview怎么后退返回 uniapp前进后退
---
在uni-app中使用了webview之后, 由于项目完全由app内嵌h5完成, 所以无法控制app的物理返回键； 要实现这个功能，最主要的是完成应用与H5 的通讯问题。
## 接受h5发来的信息 uniapp上 
首先处理uni-app: 需要在webview标签上加入接受消息的方法，通过e.detail.data 来获得消息中带来的信息
```vue
<template>
<!-- 接受web-view 的地址 message监听h5页面改变 -->
	<web-view src="http://192.168.3.7:8229/home" @message="getH5Message"></web-view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				// 定义一个空的 下面用得到 定时器给到这个一个值
                webView:'',
			}
		},
		onLoad() {
			setTimeout(()=>{
				this.operation()
			},1000)
		},
        // 原生方法  返回键操作
		onBackPress(e) {
			console.log("onBackPress e:" + JSON.stringify(e))
			// 在这里直接默认所有的返回键操作都去执行h5的方法  应用控制H5页面返回 window.myHistory() 重点
			this.webView.evalJS('window.myHistory()')
			return true; // 返回true 表示不执行返回键默认操作
		},
		methods: {
			operation() {
				// #ifdef APP-PLUS
				// 此对象相当于html5 plus里的plus.webview.webView()。
				// 在uni-app里vue页面直接使用plus.webview.webView()无效，
				// 非v3编译模式使用this.$mp.page.$getAppWebview()
				this.webView = this.$mp.page.$getAppWebview().children()[0]
				//如果是页面初始化调用时，需要 setTimeout 延时一下
				// #endif
			},
            // 接受h5发来的信息
			getH5Message(e) {
				console.log('来自webview的消息', e)
				var item = e.detail.data[0]
			}
		},
	}
</script>


```

## h5中修改
接下来处理H5页面： 要在H5页面中加入uni的SDK，只有引入了这个SDK，才可以在uni-app的webview页面中使用uni的api方法 
App 端使用 自定义组件模式 时，uni.web-view.js 的最低版为 <a href="./uniapp实现像浏览器一样前进后退的功能/uni.webview.1.5.4.js">uni.webview.1.5.2.js</a>
由于需要全局引入  所以直接在index.html操作了
```js
<script type="text/javascript" src="./uni.webview.1.5.4.js"></script>

      <script type="text/javascript">
        // 待触发 `UniAppJSBridgeReady` 事件后，即可调用 uni 的 API。如果不是一打开页面就调用 可以不用这个监听
        document.addEventListener('UniAppJSBridgeReady', function(e) {
          // 使用postMessage 方法可以发送消息到应用, 消息内容需要在data 对象中,否则webview无法接收到
          uni.postMessage({
            data: {
              message: '我是来自H5的消息' + JSON.stringify(e),
              siyecao: window.location.href,

            },
          })
          uni.getEnv(function(res) {
            console.log('当前环境：' + JSON.stringify(res));
          });
        })
        </script>

```
 ### h5 需要全局注册 最好可以使用vue的东西 所以我在app.vue写了
 ```
 mounted() {
      window.myHistory = () => {
        // window.history.go(-1)
        this.$router.go(-1)
      }
    },
 ```
