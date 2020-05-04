---
uuid: a4558889-3e0d-ea9d-8f06-b31cad0fa58e
title: vuex和全局组件传递的使用
date: 2020-03-27 21:14:50
tags: vue
category: vue
---
使用vuex中的state需要在组件中导入，计算属性中使用...mapState({name: state => state.XX.XXX.XXX})，使用方法的时候可以直接this.$store.commit("logout");就是调用mutations上的方法

vuex和全局组件传递的使用，传给vuex的时候是一个确切的值，页面通过this.$store.dispatch("",{})的属性给Action值进行整合，Action之后通过commit属性给mutations值，之后mutations直接把值给state
##  vuex
* 向vuex中传递参数
* 执行一个动作或者一个事件如：页面加载，点击。处于活跃状态等生命周期
* vuex的目录结构：
  //保存状态的：state
  /保存方法的：mutations：//单独页面，统一导入
  //计算属性：Getters；//单独页面，统一导入
  //处理异步的操作：Action；//单独页面，统一导入-Action 提交的是 mutation，而不是直接变更状态-
  //划分模块，Module


* 页面使用使用this.$store.dispatch传输给VUEX中的actions-----1：Action 提交的是 mutation，而不是直接变更状态；2：Action 可以包含任意异步操作。
Action返回值可以是一个Promise通过.then可以调用返回值-----id="0"

```
/*
*引号内是发出的事件，vuex actions中接收，
*this.index是data或者其他地方的参数。可以是任何形式的，如{}，[]
*/
this.$store.dispatch('addClick',this.index)

//Promise   -----id="0"
this.$store.dispatch('addClick',this.index).then(res => {

  //打印的是在vuex中的actions里面的resolve()  id：01
  console.log(res)
})
```

* actions接收页面传来的数据
你可以调用 context.commit 提交一个 mutation，或者通过 context.state 和 context.getters 来获取 state 和 getters

```
//传入的方法，之后给到mutations
  actions: {
    //事件名称接收addClick  //第一个参数是固定的，第二个payLoad传来的参数
    addClick(context, payLoad) {

      //返回的是上一级调用Promise的res（使用dispatch传输给VUEX中的actions）id：01
      resolve('当前数量+1')
    },
  },
```

* actions使用commit传输给VUEX中的mutations

```
//接收到参数通过commit给mutations  getTabControl(){}
actions{
   context.commit('getTabControl',payLoad)
}
```

* 也可以在页面直接传递

```
this.$store.commit('getTabControl',this.index)
/*
*引号内是发出的事件，vuex mutations中接收，
*this.index是data或者其他地方的参数。可以是任何形式的，如{}，[]
*payLoad接收，可以其他名字，但不建议
*/
```

* mutations接收actions和页面传来的参数

```
//传入的方法最好是是单一的 getTabControl
mutations: {
  //需要通过state.tabcontrol赋值给state
  /*
  *参数：
  *payLoad--传输来的值
  *state----state系统定义的第一个参数
  *
  */
    getTabControl(state, payLoad){
      state.tabcontrol = payLoad
    }
  },
```

* vuex页面上的应用

```
//导入vuex会找到Actions内定义的方法进行传入
/*
*mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性
*mapActions 辅助函数仅仅是将 store 中的 actions 映射到methods ......
*/

import {mapActions} from 'vuex'

//使用vuex
  methods : {
    //数组方式使用，还可以对象方式等
    ...mapActions([
      //tabcontrol是Actions内的一个方法，进行导入的是在计算属性内，
          'tabcontrol'
      ]),
  },

//调用vuex的数据，由于此步骤的上一步进行了导入，所以这里可以直接操作
this.tabcontrol//可以直接达到
```

async/await

```
// 假设 getData() 和 getOtherData() 返回的是 Promise

actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // 等待 actionA 完成后执行下面的
    commit('gotOtherData', await getOtherData())
  }
}

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