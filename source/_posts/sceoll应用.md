---
uuid: 823333a1-fa33-de81-fe59-c21e425aa347
title: sceoll插件应用
date: 2020-03-28 10:51:51
tags: vue插件
category: vue
---
如果滚动不好用使用contentScroll执行组件中的refresh() 或者 @load=""给scroll中组件内的图片加上这个属性之后传出来执行refresh()

##  scroll 网址：https://better-scroll.github.io/

* 安装:npm install @better-scroll/core@next --save
----------
* 其余插件介绍

	目前支持鼠标滚轮有：核心滚动
		npm install @better-scroll/mouse-wheel@next --save

	下拉动作
		npm install @better-scroll/pull-down@next --save

	样式美观的滚动条
		npm install @better-scroll/scroll-bar@next --save

	用于轮播和 swipe 效果
		npm install @better-scroll/slide@next --save

  上拉加载插件
    npm install @better-scroll/pull-up@next --save      
-------------

* 模板使用方法 
	通过ref=“scroll”
	:prode-type="3"    3为监听滚动到拿了
	:pullUpLoad="true"  需要安装插件，监听上拉加载
	@scroll="事件名"  返回参数“position”--返回顶部：
  
  ```  
  //内容滚动到一定位置返回按钮显示
    contentScroll(position) {
      this.isShowBackTop = -position.y > 1000;
    },
  这个方法返回一个true，定义一个组件为flase的时候为隐藏，ture为显示
  ```

	@pullingUp=“事件”  上拉加载事件加载更多

  ```
  data中有个列表名称所以能直接使用
  currentType：“pop”
  
  找到异步传来的函数调用列表名
  this.getHomeGoods(this.currentType);
  //异步请求，有列表名称和页码
  getHomeGoods(type) {
        //获取当前第几页
        const page = this.goods[type].page + 1;
        //异步请求
        getHomeGoods(type, page).then(res => {
          //ES6语法追加到某个数组中
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1;
          //监听连续加载
          this.$refs.scroll.finishPullUp();
        });
      }
  ```
------

* 使用
  ```
    //组件上
    <scroll class="content" ref="scroll" :probe-type="2" @scroll="contentScroll"></scroll>
    import Scroll from "@/components/common/scroll/Scroll";

        //scrol滚动
        contentScroll() {
          this.$refs.scroll.refresh();
        },
    ```
* 如果滚动不好用使用contentScroll执行组件中的refresh() 或者 @load=""给scroll中组件内的图片加上这个属性之后传出来执行refresh()
