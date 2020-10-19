---
uuid: 3e3c5d34-74d3-81cc-b9dc-397701d1f915
title: vue基础
date: 2020-03-28 17:29:25
tags: vue 基本生命周期
category: vue
---
### vue属性
data(){return:{}}
methods:{}//定义方法属性
computed:{}//计算属性
components:{}//模板注册
props:{}//获取父组件的数据
<!-- more -->
生命周期 内可以传输参数created(abc) {}

### 1.new vue() : 这是new了一个vue的实例对象；此时就会进入组件的创建过程。

### 2.Init Events & Lifecycle ：初始化组件的事件和生命周期函数；当执行完这一步之后，组件的生命周期函数就已经全部初始化好了，等待着依次去调用。

### 3.beforeCreate ：

官方说明：在实例初始化之后，数据观测(data observer) 和 event/watcher 事件配置之前被调用。

解释：这是第一个生命周期函数；此时，组件的data和methods以及页面DOM结构，都还没有初始化；所以此阶段，什么都做不了。

### 4.Init injections & reactivity ：这个阶段中，正在初始化data和methods中的数据以及方法。

### 5.created ：

官方说明：实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。

解释：这个组件创建阶段第二个生命周期函数，此时，组件的data和methods已经可以用了；但是页面还没有渲染出来；在这个生命周期函数中，我们经常会发起Ajax请求；

### 6.正在解析模板结构，把data上的数据拿到，并且解析执行模板结构汇总的指令；当所有指令被解析完毕，那么模板页面就被渲染到内存中了；当模板编译完成，我们的模板页面，还没有挂载到页面上，只是存在于内存中，用户看不到页面；

### 7.beforeMount :

官方说明：在挂载开始之前被调用：相关的 render 函数首次被调用。

解释：当模板在内存中编译完成，会立即执行实例创建阶段的第三个生命周期函数，这个函数就是beforeMount，此时内存中的模板结构，还没有真正渲染到页面上；此时，页面上看不到真实的数据，用户看到的只是一个模板页面而已；

### 8.这一步把正在内存中渲染好的模板结构替换到页面上；

### 9.mounted ：

官方说明：el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。 

解释：mounted是组件创建阶段最后的一个生命周期函数；此时，页面已经真正的渲染好了，用户可以看到真实的页面数据了；当这个生命周期函数执行完，组件就离开了创建阶段，进入到了运行中的阶段；如果大家使用到一些第三方的UI插件，而且这个插件还需要被初始化，那么，必须在mounted中来初始化插件；

### 10.组件运行汇总的生命周期函数；组件运行中的生命周期函数，会根据data数据的变化，有选择性的被触发0次货N次；

### 11.beforeUpdate ：

官方说明：数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。

解释：在执行beforeUpdate运行中的生命周期函数的时候，数据肯定是最新的；但是页面上呈现的数据还是旧的

### 12.正在根据最新的data数据，重新渲染内存中的的模板结构；并把渲染好的模板结构替换到页面上

### 13.updated ： 

官方说明：由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。

解释：页面也完成了更新，此时，data数据是最新的，同时，页面上呈现的数据也只最新的。

### 14.beforeDestroy ：

官方说明：实例销毁之前调用。在这一步，实例仍然完全可用。

解释：当执行beforeDestroy的时候，组件即将被销毁，但是还没有真正开始销毁，此时组件还是正常可用的；data、methods等数据或方法，依旧可以被正常访问

### 15.销毁过程

### 16.destroyed ：

官方说明：vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

解释：组件已完成了销毁，组件无法使用，data和methods都不可使用。

### 代码
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <input  v-model="message"/>
    <p>{{message}}</p>
    <button @click="clickfun">按钮</button>
</div>
</body>
<script>
    var myVue = new Vue({
        el: '#app',
        data: function () {
            return {
                message: 'XXXX'
            };
        },
        beforeCreate: function () {
            console.group('beforeCreate 创建前状态===============》');
            var state = {
                'el': this.$el,
                'data': this.$data,
                'message': this.message
            }
            console.log(state);
        },
        created: function () {
            console.group('created 创建完毕状态===============》');
            var state = {
                'el': this.$el,
                'data': this.$data,
                'message': this.message
            }
            console.log(state);
        },
        beforeMount: function () {
            console.group('beforeMount 挂载前状态===============》');
            var state = {
                'el': this.$el,
                'data': this.$data,
                'message': this.message
            }
            console.log(this.$el);
            console.log(state);
        },
        mounted: function () {
            console.group('mounted 挂载结束状态===============》');
            var state = {
                'el': this.$el,
                'data': this.$data,
                'message': this.message
            }
            console.log(this.$el);
            console.log(state);
        },
        beforeUpdate: function () {
            console.group('beforeUpdate 更新前状态===============》');
            var state = {
                'el': this.$el,
                'data': this.$data,
                'message': this.message
            }
            console.log(this.$el);
            console.log(state);
            console.log('beforeUpdate == ' + document.getElementsByTagName('p')[0].innerHTML);
        },
        updated: function () {
            console.group('updated 更新完成状态===============》');
            var state = {
                'el': this.$el,
                'data': this.$data,
                'message': this.message
            }
            console.log(this.$el);
            console.log(state);
            console.log('updated == ' + document.getElementsByTagName('p')[0].innerHTML);
        },
        beforeDestroy: function () {
            console.group('beforeDestroy 销毁前状态===============》');
            var state = {
                'el': this.$el,
                'data': this.$data,
                'message': this.message
            }
            console.log(this.$el);
            console.log(state);
        },
        destroyed: function () {
            console.group('destroyed 销毁完成状态===============》');
            var state = {
                'el': this.$el,
                'data': this.$data,
                'message': this.message
            }
            console.log(this.$el);
            console.log(state);
        },
        methods: {
            clickfun() {
                myVue.$destroy()
            }
        
        }
        
        
    });
</script>
</html>
```

### 组件传递
props：父传子
ref：子传父//定义到父组件模板上ref=“scroll”  通过this.$refs.scroll   
emit：子传父//子组件发出事件this.$emit("scroll", position);   父组件接收@scroll=""

### 事件监听基本使用
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

### 修饰符
stop；//可以阻止事件冒泡
prevent；//阻止系统自动提交
.enter；//键盘按下回车事件
.native；//组件定义事件的时候使用@cilkc.native=""
.once；//只执行一次的事件

### 插槽语法
slot name=”left“单个替换
  html使用的时候需要更具名字进行替换例子：
  <slot name="left"><button >左边</button></slot>
  <slot name="btn"><button >按钮</button></slot>
  <cpn><button slot="btn">1</button></cpn>
  如果html没有名字，替换模板没有名字的数据---注意模板是给slot一个name=“”    使用的时候是给buttom slot=“”的

:img加载完成
  @load=“方法”

### 数据传输给对象需要新加建给对象
this.res.data = res//data是对象中新加的值

### 数据传输给数组的时候需要push进去，或者用数组其他方法