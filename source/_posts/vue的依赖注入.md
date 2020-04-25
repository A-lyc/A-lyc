---
uuid: 53631cbf-9718-162e-8d54-ba6af932dc1b
title: vue的依赖注入provide，inject
date: 2020-04-24 20:02:39
tags: vue
category: vue
---
vue中使用很频繁的组件可以使用依赖注入的形式去编写，就是在一个公共文件，或者根组件app.vue中定义一个样式，之后使用provide(){return {}}的方法进行导出，之后在子组件中使用inject:[""]进行导入，下面是举例子说明


App.vue
```
<script>
export default {
  name: "app",
  //依赖注入//依赖注入，给全局注册一个依赖
  provide(){
    return {
      app:this,//当前的额this就是app全局可调用app.show即可调用show的方法
    }
  },
  data () {
    return {
      imageModel: false
    }
  },
  components: {},
  methods: {
    show(){
      this.imageModel = true
    },
    confirm(){
      this.imageModel = false
    },
    hide(){
      this.imageModel = false
    }
  },
};
</script>

```

子组件：使用注入的依赖
```
export default {
inject:['app'],
methods:{
  isShow(){
    console,log(this.app.show)//这个值是上面App.vue中定义的一个函数
    console,log(this.app.imageModel)// false   因为上文中data中定义的是一个false
  }
}
}
```