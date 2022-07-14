---
uuid: 823333a1-fa33-de81-fe59-c21e425aa347
title: new Promise 理解
date: 2022-07-14 10:51:51
tags: Promise
category: Promise
---
```js
  function getData(){
        setTimeout(function(){
          var name = 'tom';
          return name;
        },1000)
    }
console.log(getData());  // undefined; 一开始以为能获取到tom
```
· 需要回调函数
```js
   function getData(callback){    
      setTimeout(function(){
          var name = 'tom';
          callback(name);
      },1000)
    }
    getData(function(val){
          console.log(val);    // tom
    }); 
```

· 使用promise方法 获取异步方法里面的数据
```js
 var p = new Promise((resolve,reject)=>{
          setTimeout(function(){
              var count = 0;

              if(count){
                  resolve( count );
              }else{
                  reject( count );
              }
          },1000)
    });

    p.then(res=>{           //异步调用
        console.log(res);   
    },err=>{
        console.log(err)     
    })
    p.then().calch()
    console.log(1);     //比then先执行，也就证明then是异步调用
```
· then链式调用
```js
p.then(res=>{
    console.log(res);
    return new Promise((resolve,reject)=>{
              resolve('需再return promise对象，才能继续then调用')
    })
})
.then(res=>{
     console.log(res) // 需要return p.....
})
```
err & catch 谁会捕捉reject?
```js
p.then(res=>{
}, err=>{
    // 当同时存在，会直接走then的err回调函数, catch不执行
    console.log('then的', err);   
}).catch(err=>{
    console.log('cathc的', err);
})
```
· 快速获得promise对象
 ```js
 //传入的参数非promise对象，则返回resolve/reject的promise对象
  let p1 = Promise.resolve('abc');      
  let p3 = Promise.reject('wrong')；

  //传入的参数promise对象，则结果根据resolve / reject
  let p2 = Promise.resolve( 
              new Promise((resolve,reject)=>{
                  reject('abc')
              }) 
  );

  const [nameOptions,methodOptions] = await Promise.all([
          getDictionaries({code:'BILL_BUSINESS_NAME',status:1}),
          getDictionaries({code:'BILL_PAYMENT_METHOD',status:1}),
  ]);
  p.then(res=>{
      if(res.code === 200){

          // 有时候业务不需要处理逻辑,就可以很方便直接返回promise对象。
          return Promise.resolve( res.data );
          // 不需要↓这样复杂;
          // return new Promise((resolve,reject){resolve( res.data )}); 
      }
  }).then(data=>{
      console.log('成功啦!!')
      console.log(data);
  });
 ```

· promise对象提供了then，catch, all, race方法
![查入图片](./1.webp) 

![查入图片](./2.webp) 

· 注意:
1.all方法的传递参数需要是数组;
2.all方法的所有promise是并发执行(同时执行);
3.任何一个promise返回reject，就会中断去执行catch
![查入图片](./3.webp) 

· ES7提供的async & await处理异步函数
1.async使得函数变成异步,添加后依然同普通函数使用;
2.await操作符必须在async函数内使用;
3.await操作符等待的函数必须要返回的一个promise对象;
4.promise返回的是reject, await拿不到, 需要配合try catch来捕捉reject;
```js
async function test(){
  try{
    var status = await Promise.resolve(200);
    console.log(status);      // 200
    var notFound = await Promise.reject(404);
     console.log(notFound);   // 没有打印; ↓catch会捕捉到reject;

     console.log(55)          // 没有打印; 当前面返回reject，后续不再执行;
  
  }catch(err){
    console.log(err);         // 404
  }
}
test();
console.log(1)    // 比test函数先执行,也就证明test是异步函数;
```

作者：Peter_2B
链接：https://www.jianshu.com/p/42c5eea9b698
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。