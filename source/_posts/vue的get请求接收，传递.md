---
uuid: 431cde16-c6c7-2c6e-529e-a06f6bd31b8f
title: vue axios的get请求接收，传递
date: 2020-04-11 20:11:28
tags:
category:
---

上次文章有封装好的axios 使用vue封装好的网络请求，可以通过引入的方式进行网络请求，就是引用：
```
import { getDetail, Goods, DetailActive, DetailInfo } from "@/network/detail";
```
在methods里面处理好之后，在页面加载完成发出网络请求，
如何接收网络请求，上次封装有的
使用promise.then(()=>{}).catch(()=>{})的方式。之后使用this保存值

如何返回值传入的时候有变动的参数进行拼接，使用params：{type，page....}等进行拼接
```
export function getDetail(iid){
  return request({
    url: '/detail',
    params:{
      iid
    }
  })
}
```
GET：在methods里面处理的时候可以：异步函数（参数1，参数2）进行传输参数
```
getHomeGoods(type) {
      let page = 1
      getHomeGoods(type, page).then(res => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;
      });
    }
```

结合router使用，比如点击进入详情页，可以通过改变router的值改变你url
```
//点击传入
this.$router.push('/detail/'+this.goodsItem.iid)
 
//router接收
  path: '/detail/:iid',
  name: 'Detail',
  component: Detail

//打开页面找到url上拼接的id
 this.iid = this.$route.params.iid;

//发出网络请求，把iid传入，请求iid的数据
getDetail(this.iid).then(res => {})

```

之后页面加载前取到url上的iid，获取方法：this.iid = this.$route.params.iid;（当前页面：route）之后使用把值代入函数的参数内传输给后端，（获取的时候要有一个params：{type，page....}）拼接的值，然后后端返回这个链接随对应的数据


