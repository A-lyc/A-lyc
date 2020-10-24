---
uuid: 3e3c5d34-74d3-81cc-b9dc-397701d1f915
title: vue基础
date: 2020-03-28 17:29:25
tags: vue 基本生命周期
category: vue
---
### vue属性
methods:{}//定义方法属性
computed:{}//计算属性
watch:{}//监听属性变化
components:{}//模板注册
props:{}//获取父组件的数据
<!-- more -->
生命周期 内可以传输参数created(abc) {}
使用前导入：
```vue
import { ref, computed, watch, getCurrentInstance,
onMounted,onRenderTracked,onRenderTriggered,
onBeforeMount,onBeforeUpdate,onUpdated,onBeforeUnmount,onUnmounted,
onErrorCaptured} from "vue";
```
### setup(){} ：生命周期函数需要在这个函数内执行
两种形式的生命周期函数可以共存（当然实际使用的时候最好只选用一种），它们都会被执行。Composition API形式的生命周期函数都是在 setup 方法中被调用注册。
最后，在实际的开发过程中，请注意一下Options API形式的组件生命周期钩子和Composition API之间的实际对应关系：

### beforeMount  3.0=> onBeforeMount

官方说明：在挂载开始之前被调用：相关的 render 函数首次被调用。

解释：当模板在内存中编译完成，会立即执行实例创建阶段的第三个生命周期函数，这个函数就是beforeMount，此时内存中的模板结构，还没有真正渲染到页面上；此时，页面上看不到真实的数据，用户看到的只是一个模板页面而已；
 
### mounted 3.0=> onMounted

官方说明：el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。 

解释：mounted是组件创建阶段最后的一个生命周期函数；此时，页面已经真正的渲染好了，用户可以看到真实的页面数据了；当这个生命周期函数执行完，组件就离开了创建阶段，进入到了运行中的阶段；如果大家使用到一些第三方的UI插件，而且这个插件还需要被初始化，那么，必须在mounted中来初始化插件；

### beforeUpdate 3.0=> onBeforeUpdate

官方说明：数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。

解释：在执行beforeUpdate运行中的生命周期函数的时候，数据肯定是最新的；但是页面上呈现的数据还是旧的


### updated 3.0=> onUpdated

官方说明：由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。

解释：页面也完成了更新，此时，data数据是最新的，同时，页面上呈现的数据也只最新的。

### beforeDestroy 3.0=> onBeforeUnmount

官方说明：实例销毁之前调用。在这一步，实例仍然完全可用。

解释：当执行beforeDestroy的时候，组件即将被销毁，但是还没有真正开始销毁，此时组件还是正常可用的；data、methods等数据或方法，依旧可以被正常访问

### destroyed 3.0=> onUnmounted

官方说明：vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

解释：组件已完成了销毁，组件无法使用，data和methods都不可使用。

### errorCaptured 3.0=> onErrorCaptured

### 代码
```vue
<template>
  <div class="about">
    <h1>test count: {{ count }}</h1>
    <h1>test: {{ test }}</h1>
    <h1>computed: {{ doubleCount }}</h1>
    <h1>vuex: {{ a }}</h1>
    <h1 @click="getgreet">vue3.0的computed和watch，使用方式</h1>
    <button @click="add('1123456465645465')">增加</button>
    <button @click="update('1123456465645465')">更改</button>
  </div>
</template>

<script>
import { ref, computed, watch, getCurrentInstance,
onMounted,onRenderTracked,onRenderTriggered,
onBeforeMount,onBeforeUpdate,onUpdated,onBeforeUnmount,onUnmounted,
onErrorCaptured} from "vue";

export default {
  setup() {
       const { ctx } = getCurrentInstance(); // 获取当前实例
    onBeforeMount(()=>{
      console.log("在挂载开始之前被调用")
    })
    onBeforeUnmount(()=>{
      console.log("实例销毁之前调用")
    })
    onBeforeUpdate(()=>{
      console.log(ctx)
      console.log("数据更新时调用")
    })
    onUnmounted(()=>{
      console.log("组件已完成了销毁")
    })
    onErrorCaptured(()=>{
      console.log("在错误捕获")
    })
    onUpdated(()=>{
      console.log(ctx)
      console.log("页面也完成了更新")
    })
    onMounted(() => {
      console.log("挂载后 >>>>>>01");
    });
    onRenderTracked((e) => {
      console.log('渲染跟踪');
      console.log(e);
    });
    onRenderTriggered((e) => {
      console.log('渲染 - 触发')
      console.log(e);
    });
    // 页面加载的时候触发
    const count = ref(0);
 
    console.log(getCurrentInstance());
    console.log(ctx.$router.currentRoute.value); // 获取路由
    const a = computed(() => ctx.$store.state.test.a); // 计算属性获取vuex上的属性
    const update = () => {
      // 修改vuex的信息
      ctx.$store.commit("setTestA", count.value * 10);
      console.log(ctx.$store.state.test.a);
    };
    let test = ref("我们都是好孩子"); // 定义test默认显示内容
    const add = () => {
      // 点击动作
      test.value = "我是好人"; // 修改值
      count.value++; // count加一
    };
    // function time() {
    //   setInterval(()=>{
    //     count.value++
    //   },100)
    // }
    // time()
    watch(
      () => {
        // 页面加载就读取这个信息 监听属性的变化
        console.log("---- 1 ----");
        console.log(count.value);
        count.value;
      },
      (val) => {
        console.log("---- 2 ----");
        console.log(`count is ${val}`);
      }
    );
    const doubleCount = computed(() => {
      // 计算属性获取 count.value * 2
      return count.value * 2;
    });
  
    return {
      // 返回定义的对象
      count,
      test,
      doubleCount,
      add,
      a,
      update,
    };
  },
   mounted() {
     // 这个比上面的on要晚
        console.log('挂载后 >>>>>>02')
    },
  methods: {
    getgreet() {
      console.log("---- methods的点击动作 ----");
      console.log(this.update);
    },
  },
};
</script>
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