---
uuid: 6a7ffb39-0dd3-1f1a-d30f-df238e382c2b
title: nuxt-asyncData异步数据
date: 2020-07-01 11:38:54
tags:
category:
---
asyncData:参数：
asyncData (ctx) {
    ctx.app // 根实例
    ctx.route // 路由实例
    ctx.params  //路由参数
    ctx.query  // 路由问号后面的参数
    ctx.error   // 错误处理方法
  }
{ params } = url上的/_.vue 参数：如果你定义一个名为_slug.vue的文件，您可以通过context.params.slug来访问它


```html
<template>
  <div>
    Index {{ username }}
  </div>
</template>

<script>
export default {
  name: "Index",
  async asyncData () {
//定义一个对象，上面可以访问的 - 类似于data
    const asyncData = {};
    await new Promise((resolve, reject) => {
      setTimeout(() => {
//向对象内添加参数  所以使用的时候直接username可以使用
        asyncData.username = 'John Smith';
        resolve();
      }, 2000)
    });
    return asyncData;
  }
};
</script>
```

使用 -- 
直接在页面上使用下面的代码就行了
```$vue
{{ username }}
```

