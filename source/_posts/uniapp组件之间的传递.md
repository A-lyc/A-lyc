---
uuid: 2e13fe43-5e09-4bdd-e396-0e01f9bce214
title: uniapp组件之间的传递
date: 2020-04-04 13:25:06
tags: uniapp
category: uniapp
---
uniapp里面的组件之间传递信息数据，如何接收数据

####  父组件传递给子组件，使用popst：{}和vue一样传递方式

```
//父组件传递mag给子组件，mag定义到实例上的
<view :magess="mag"></view>

//子组件接收，实例上接收,和定义在data中的方法一样使用
popst:{
  magess:{
    type:Objest,
    default(){
      return {}
    }
  }
}

```

####  子组件传递给父组件this.$emit

```
//z子组件定义事件$emit()发出事件
this.$emit('magess',{name:name,aeg:age})

//父组件接收事件
<view @magess="mag"></view>

//操作传来的事件信息
mag(e){
console.log(e)
}
```

####  uni.$emit和uni.$on页面之间传递应该是可以传输vue之间的页面，保证传输tab的页面
uni.$off 销毁
```
//页面上定义事件
uni.$emit('magess',{name:name,aeg:age})

//另外的页面进行接收，最好使用箭头函数
uni.$on('magess',(e) => {
  console.log(e)
  //这里的this可以直接访问data
})

```

### 全局事件总件
定义一个js文件，文件内是全局事件触发存储的，类似于vuex
> 实例
在 uni-app 项目根目录下创建 common 目录，然后在 common 目录下新建 helper.js 用于定义公用的方法。
``` 
const now = Date.now || function () {  
    return new Date().getTime();  
};  
const isArray = Array.isArray || function (obj) {  
    return obj instanceof Array;  
};  

export default {  
    websiteUrl,  
    now,  
    isArray  
}
```

接下来在 pages/index/index.vue 中引用该模块
```
<script>  
    import helper from '../../common/helper.js';  

    export default {  
        data() {  
            return {};  
        },  
        onLoad(){  
            console.log('now:' + helper.now());  
        },  
        methods: {  
        }  
    }  
</script>
```
这种方式维护起来比较方便，但是缺点就是每次都需要引入。

### 全局变量globalData
小程序中有个globalData概念，可以在 App 上声明全局变量。 Vue 之前是没有这类概念的，但 uni-app 引入了globalData概念，并且在包括H5、App等平台都实现了。
在 App.vue 可以定义 globalData ，也可以使用 API 读写这个值。
globalData支持vue和nvue共享数据。
globalData是一种比较简单的全局变量使用方式。
定义：App.vue

```
<script>  
    export default {  
        globalData: {  
            text: 'text'  
        },  
        onLaunch: function() {  
            console.log('App Launch')  
        },  
        onShow: function() {  
            console.log('App Show')  
        },  
        onHide: function() {  
            console.log('App Hide')  
        }  
    }  
</script>  

<style>  
    /*每个页面公共css */  
</style>  
```
js中操作globalData的方式如下：
赋值：getApp().globalData.text = 'test'
取值：console.log(getApp().globalData.text) // 'test'



修改首页数据
getCurrentPages() 函数用于获取当前页面栈的实例，以数组形式按栈的顺序给出，第一个元素为首页，最后一个元素为当前页面。（首页点击之后进入一个页面，这个页面点击之后进入下一个，这个函数会生成三个，以此类推），当点击底部的时候是一底部的为首页进入的 