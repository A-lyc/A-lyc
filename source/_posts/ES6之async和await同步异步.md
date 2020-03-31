---
uuid: 02925e5e-3d14-3f6e-430e-de8b10b28bac
title: ES6之async和await同步异步
date: 2020-03-30 16:42:45
tags: ES6
category: ES6
---
async和await是用来处理异步的。即你需要异步像同步一样执行，需要异步返回结果之后，再往下依据结果继续执行。
async 是“异步”的简写，而 await 可以认为是 async wait 的简写。
async 用于申明一个 function 是异步的，而 await 用于等待一个异步方法执行完成。
async 它用的是try/catch 来捕获异常，把await 放到 try 中进行执行，如有异常，就使用catch 进行处理。
async返回值是一个Promise 对象，也就是说可以调用then()

```
async
async function testAsync() {
    return "hello async";
}

const result = testAsync();
console.log(result);
```
打印输出的是一个Promise 对象，async 函数会返回一个 Promise 对象。
在最外层不能用 await 获取其返回值的情况下，使用 then() 链来处理这个 Promise 对象。
```
testAsync().then(v => {
    console.log(v);    // 输出 hello async
});
```
当 async 函数没有返回值时，返回 Promise.resolve(undefined)

await
await只能放在async函数内部使用

await 用于一个异步操作之前，表示要“等待”这个异步操作的返回值。
await 也可以用于一个同步的值。

如果它等到的不是一个 Promise 对象，那 await 表达式的运算结果就是它等到的东西。
如果它等到的是一个 Promise 对象，await 就会阻塞后面的代码，等着 Promise 对象 resolve，然后得到 resolve 的值，作为 await 表达式的运算结果。

同步代码

const a = await 'hello world'
// 相当于
const a = await Promise.resolve('hello world');
// 所以直接写同步代码即可，不需要await关键字
const a = 'hello world';
异步代码

// 2s 之后返回双倍的值
function doubleAfter2seconds(num) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(2 * num)
        }, 2000);
    })
}

async function testResult () {
    let result = await doubleAfter2seconds(30);
    console.log(result);
}

testResult();
// 2s 之后，输出了60. 
执行顺序
案例一
// 2s 之后返回双倍的值
function doubleAfter2seconds(num) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(2 * num)
        }, 2000);
    })
}

async function testResult () {
    console.log('内部调用前') // 2
    let result = await doubleAfter2seconds(30);
    console.log(result); // 4
    console.log('内部调用后') // 5
}

console.log('外部调用前') // 1
testResult();
console.log('外部调用后') // 3
// --- 依次输出
// 外部调用前
// 内部调用前
// 外部调用后
// --- 2s 之后输出
// 60
// 内部调用后
分析一下上面的执行顺序：
1、首先打印输出外部调用前，同步代码，顺序执行。
2、然后调用方法testResult()，打印输出内部调用前，同步代码，顺序执行。
3、再执行异步方法doubleAfter2seconds，
　1>如果没用await关键字，此后的执行顺序应该是
　　内部调用后，外部调用后，2s 之后输出60
　　因为异步方法不阻塞其他代码的执行，最后再输出60
　2>这里使用了await关键字，所以到这里后会等待异步返回结果，再往下执行。
4、当testResult函数内部await阻塞执行后，不会影响到testResult函数外面

async 函数调用不会造成阻塞，它内部所有的阻塞都被封装在一个 Promise 对象中异步执行。

所以，在调用testResult函数后，会继续向下执行，打印输出外部调用后
5、当2s之后，异步函数doubleAfter2seconds执行完成，返回结果，
打印输出60
6、因为await将异步变成同步，所以在输出60后，同步执行，再输出内部调用后

案例二
代码

console.log("1")
异步处理函数：console.log（2）
console.log(3)
结果

正常情况 132
用async await 123
例子
// 2s 之后返回双倍的值
function doubleAfter2seconds(num) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(2 * num)
        }, 2000);
    })
}

async function testResult () {
    let first = await doubleAfter2seconds(10);
    let second = await doubleAfter2seconds(20);    
    console.log(first + second);
}
错误处理
方式一 统一处理
// 2s 之后返回双倍的值

```
//不使用async
function doubleAfter2seconds(num) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(2 * num)
        }, 2000);
    })
}


//使用async
async function testResult () {
    let first = await doubleAfter2seconds(10);
    let second = await doubleAfter2seconds(20);    
    let res = first + second;
    return res;
}

testResult().then(res => {
    console.log(res);      
}).catch(error => {
    console.log(error);     
});
```

方式二 try...catch
// 2s 之后返回双倍的值

```
function doubleAfter2seconds(num) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(2 * num)
        }, 2000);
    })
}

async function testResult () {
    try {
        let first = await doubleAfter2seconds(10);
        let second = await doubleAfter2seconds(20);    
        let res = first + second;
        return res;
    } catch (error) {
        console.log(error);
    }
}

testResult()
在接口中使用(axios)
created () {
    this.init()
},
methods: {
    async init () {
      try {
          let first = await this.getOne();
          let second = await this.getTwo();    
          let res = first + second;
          console.log(res);
      } catch (error) {
          console.log(error);
      }        
    },
    getOne () {
        const params = {name: 'one'}
        return new Promise((resolve, reject) => {
            axios.get('/one', { params}).then((res) => {
                if (res.status === 200) {
                    resolve(res)
                }
            }).catch((err) => {
                reject(err)
            })
        })
    },
    getTwo () {
        const params = {name: 'two'}
        return new Promise((resolve, reject) => {
            axios.get('/two', { params}).then((res) => {
                if (res.status === 200) {
                    resolve(res)
                }
            }).catch((err) => {
                reject(err)
            })
        })
    },
},
```

它用的是try/catch 来捕获异常，把await 放到 try 中进行执行，如有异常，就使用catch 进行处理。