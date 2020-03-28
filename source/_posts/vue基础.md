---
uuid: 3e3c5d34-74d3-81cc-b9dc-397701d1f915
title: vue基础
date: 2020-03-28 17:29:25
tags: vue
category: vue
---
vue属性
data(){return:{}}
methods:{}//定义方法属性
computed:{}//计算属性
components:{}//模板注册
props:{}//获取父组件的数据

生命周期
created() {},//生命周期 - 创建完成（可以访问当前this实例）
mounted() {},//生命周期 - 挂载完成（可以访问DOM元素）
beforeCreate() {}, //生命周期 - 创建之前
beforeMount() {}, //生命周期 - 挂载之前
beforeUpdate() {}, //生命周期 - 更新之前
updated() {}, //生命周期 - 更新之后
beforeDestroy() {}, //生命周期 - 销毁之前
destroyed() {}, //生命周期 - 销毁完成
activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发

组件传递
props：父传子
ref：子传父//定义到父组件模板上ref=“scroll”  通过this.$refs.scroll   
emit：子传父//子组件发出事件this.$emit("scroll", position);   父组件接收@scroll=""

事件监听基本使用
v-once
v-html=“”
v-text=“”
v-cloak
v-pre
v-bind
v-on:
v-for=“变量 in 数组或对象”
v-if/v-else
v-show
ladet，点击输入框会有一个聚焦状态
v-model=“”数据的双向绑定
事件@input=“”

修饰符
stop；//可以阻止事件冒泡
prevent；//阻止系统自动提交
.enter；//键盘按下回车事件
.native；//组件定义事件的时候使用@cilkc.native=""
.once；//只执行一次的事件

插槽语法
slot name=”left“单个替换
  html使用的时候需要更具名字进行替换例子：
  <slot name="left"><button >左边</button></slot>
  <slot name="btn"><button >按钮</button></slot>
  <cpn><button slot="btn">1</button></cpn>
  如果html没有名字，替换模板没有名字的数据---注意模板是给slot一个name=“”    使用的时候是给buttom slot=“”的

:img加载完成
  @load=“方法”