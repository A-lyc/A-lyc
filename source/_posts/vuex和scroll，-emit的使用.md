---
uuid: a4558889-3e0d-ea9d-8f06-b31cad0fa58e
title: vuex和scroll，$emit，popst的使用
date: 2020-03-27 21:14:50
tags: vue
category: vue
---

##  vuex
* 向vuex中传递参数
* 执行一个动作或者一个事件等
* commit传输给mutations

```
this.$store.commit('getTabControl',this.index)
/*
*引号内是发出的事件，vuex mutations中接收，
this.index是data或者其他地方的参数。可以是任何形式的，如{}，[]
*/
```


* dispatch传输给actions

```
this.$store.dispatch('addClick',this.index)
/*
*引号内是发出的事件，vuex actions中接收，
*this.index是data或者其他地方的参数。可以是任何形式的，如{}，[]
*/
//这个可以传输一个Promise
this.$store.dispatch('addClick',this.index).then(res => {
  console.log(res)
})
```


* vuex中接收

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
    }//需要通过这个赋值给state
  },

  //传入的方法随意，之后给到mutations
  actions: {
    tabcontrols(context, payLoad) {
      context.commit('getTabControl',payLoad)
    },//接收到参数通过commit给mutations
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
    ...mapActions([
          'tabcontrol'
      ]),
  },

//调用vuex的数据
this.$store.state.tabcontrol
```

##  scroll 网址：https://better-scroll.github.io/

* 安装:npm install @better-scroll/core@next --save

```
<scroll class="content" ref="scroll" :probe-type="2" @scroll="contentScroll"></scroll>
import Scroll from "@/components/common/scroll/Scroll";
     //scrol滚动
    contentScroll() {
      this.$refs.scroll.refresh();
    },
```
* 如果滚动不好用使用contentScroll执行组件中的refresh() 或者 @load=""给scroll中组件内的图片加上这个属性之后传出来执行refresh()


## $emit子组件定义事件，父组件接收

* 子组件：

```
  methods:{
    addToCart(index){
      this.$emit('addCart',index)
    },
  }
```

* 父组件

```
 <detail-bottom-bar @addCart="addToCart"></detail-bottom-bar>
 //addToCart:可以自己定义事件内容,还可以取到传来的参数
```


## popst父组件传数据，子组件接收

* 父组件：

```
//父组件把goods传入子组件
:good="goods"
//good和子组件关联的
```

* 子组件

```
props: {
  //子组件的good是父组件传来的goods
    good:{
        type:Array,
        //数组和对象默认值是一个函数
        default(){
            return []
        }
    }
},
```