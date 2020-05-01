---
uuid: 3101d82a-9f9f-6476-2103-383b39819358
title: router内的query和params
date: 2020-04-30 10:38:30
tags: vue
category: vue
---
this.$route.query和this.$route.params的使用：this.$route.query根据path来找查在url上有显示的，params根据name来找查在url上没有显示

### 一、this.$route.query的使用,

1、router/index.js
```
{
path:'/mtindex',
component: mtindex,
//添加路由
    children:[
    {
        path:':shopid',
        component:guessdetail
    }
    ]
},
```

2、传参数
```
this.$router.push({
      path: '/mtindex/detail', 
      query:{shopid: item.id}
        });

```

3、获取参数
```
this.$route.query.shopid
```

4、url的表现形式(url中带有参数)
```
http://localhost:8080/#/mtindex/detail?shopid=1

```

### 二、this.$route.params

1、router/index.js
```
{
    path:'/mtindex',
    component: mtindex,
    //添加路由
        children:[
        {
            path:"/detail",
            name:'detail',
            component:guessdetail
        }
        ]
    },
```

2、传参数（ params相对应的是name query相对应的是path）
```
this.$router.push({
      name: 'detail', 
      params:{shopid: item.id}
        });
```

3、获取参数
```
this.$route.params.shopid
```

4、url的表现形式(url中没带参数)
```
http://localhost:8080/#/mtindex
```

原文链接：https://www.jianshu.com/p/5deb7e90af76
