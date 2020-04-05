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

####  子组件传递给父组件

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

####  页面之间传递

```
//页面上定义事件
uni.$emit('magess',{name:name,aeg:age})

//另外的页面进行接收，最好使用箭头函数
uni.$on('magess',(e) => {
  console.log(e)
  //这里的this可以直接访问data
})

```

修改首页数据
getCurrentPages() 函数用于获取当前页面栈的实例，以数组形式按栈的顺序给出，第一个元素为首页，最后一个元素为当前页面。（首页点击之后进入一个页面，这个页面点击之后进入下一个，这个函数会生成三个，以此类推），当点击底部的时候是一底部的为首页进入的 