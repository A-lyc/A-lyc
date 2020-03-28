---
uuid: a4558889-3e0d-ea9d-8f06-b31cad0fa58e
title: vuex和全局组件传递的使用
date: 2020-03-27 21:14:50
tags: vue
category: vue
---
vuex和全局组件传递的使用
##  vuex
* 向vuex中传递参数
* 执行一个动作或者一个事件如：页面加载，点击。处于活跃状态等生命周期
* vuex的目录结构：
  //保存状态的：state
  /保存方法的：mutations：//单独页面，统一导入
  //计算属性：Getters；//单独页面，统一导入
  //处理异步的操作：Action；//单独页面，统一导入
  //划分模块，Module


* 使用commit传输给VUEX中的mutations

```
this.$store.commit('getTabControl',this.index)
/*
*引号内是发出的事件，vuex mutations中接收，
this.index是data或者其他地方的参数。可以是任何形式的，如{}，[]
*/
```


* 使用dispatch传输给VUEX中的actions

```
/*
*引号内是发出的事件，vuex actions中接收，
*this.index是data或者其他地方的参数。可以是任何形式的，如{}，[]
*/
this.$store.dispatch('addClick',this.index)

//这个可以传输一个Promise
this.$store.dispatch('addClick',this.index).then(res => {

  //打印的是在vuex中的actions里面的resolve()
  console.log(res)
})
```


* vuex中接收传来的参数

```
export default new Vuex.Store({

//不可以直接修改state内的值
  state: {
    tabcontrol:'pop',//接收到的实际参数,其他页面可应用的参数
  },

//传入的方法最好是是单一的
mutations: {
    getTabControl(state, payLoad){
      state.tabcontrol = payLoad
    }//需要通过state.tabcontrol赋值给state
  },

  //传入的方法，之后给到mutations
  actions: {
    tabcontrols(context, payLoad) {

      //返回的是上一级调用Promise的res（使用dispatch传输给VUEX中的actions）
      resolve('当前数量+1')
      
      //接收到参数通过commit给mutations
      context.commit('getTabControl',payLoad)
    },
  },
  modules: {
  }
})
```


* vuex应用

```
//导入vuex
import {mapActions} from 'vuex'

//使用vuex
  computed: {
    //数组方式使用，还可以对象方式等
    ...mapActions([
          'tabcontrol'
      ]),
  },

//调用vuex的数据
this.$store.state.tabcontrol
```

##  $bus  自定义后缀

min.js定义一下

```
Vue.prototype.$bus = new Vue()
```

传出：

```
//使用$bus.$emit传出
this.$bus.$emit('itemImages',parameter)
```

接收：

```
接收
  this.$bus.$on("itemImages", () => {});
```