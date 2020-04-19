---
uuid: cf43ee82-9d89-f2bd-da3d-6f24b15ef25e
title: 计算属性中的get和set以及watch的router，本地缓存问题
date: 2020-04-16 20:38:15
tags: vue
category: vue
---
在vue中计算属性有两个值，一个是获取，一个是写入，大部分使用的都是计算属性中的获取功能，少部分使用写入这个功能，这个本人不太懂，知道有这个东西，它可以改变获取的值，不走data的逻辑，set可以间接修改get里面的值，页面直接修改get内的值是修改不了的
```
computed: {
    slideMenusActive: {
      get() {
        return this.tabBars.list[this.tabBars.active].submenuActive || "0";
      },//获取
      set(val) {
        console.log(val)
        this.tabBars.list[this.tabBars.active].submenuActive = val;
      }//写入
    },
    slideMenus() {
      return this.tabBars.list[this.tabBars.active].submenu || [];
    }
  }
```

在watch中使用router的时候需要
```
watch: {
  //获取$route的路由，to是当前from是最后
     '$route'(to,from){
        //下面方法是本地存储--刷新之后还是当前页面 localStorage.setItem('navActive',JSON.stringify({to:'',from:''})
        localStorage.setItem('navActive',JSON.stringify({
           top:this.tabBars.active,
           left:this.slideMenusActive
           
        }))
        console.log(to,from)
      this.getRouterBran()
     }
  }
```

上面有本地存储，就要有获取才对
```
    //初始化
    __initNavBar(){
      //获取本地存储   自定义的值要一样navActive
      let r =  localStorage.getItem('navActive')
      if(r){
        r = JSON.parse(r)
        this.tabBars.active = r.top
        this.slideMenusActive = r.left
      }
     
    }
```
