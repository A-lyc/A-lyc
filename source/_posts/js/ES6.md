---
title: ES6
date: 2020-08-19 20:14:36
tags: ES6
category: ES6
---
定义变量 let 
定义常量 const：声明一个只读的常量。一旦声明，常量的值就不能改变。
魔法字符串 '`${}`'
结构语法 let {data} = res;let [a,b,c] = arr
函数默认语法 f function f( x = {}){return x} // 默认空对象
遍历追加数组 arr.push(...array)

### class
```js
class Calc {
  constructor(){
    console.log('每次都会走这个地方，可以不设置这个函数');
  }
  add(a,b){
    console.log('这里是使用到的函数')
    return a + b
  }
}
var c = new Calc()
c.d = 10
console.log(c)// {d:10}
console.log(c.add(1,2))// 此方法在原型上 __proto__ 属性上的类
```
### function的默认值
```js 
function(x=1){
  console.log(x)// 1
}
```
### try{}catch(e){}
  捕捉try内的一个错误之后执行catch内的方法
### 引入global作为顶层对象
```js
// CommonJS的写法
var global = require('system.global')();

// ES6模块的写法
import global from 'system.global'; global();
```


### 变量结构赋值
```js
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3
```
### 数组：new Set()
一个数组的语法 add()数组去重
首先new一个Set 
```js
const arr = new Set()
[{id:1},{id:2},{id:1}].forEach(x => s.add(x.id))
console.log(s) //Set(2) {1, 2}
```
#### 去除数组的重复成员
[...new Set(array)]
add(value)：添加某个值，返回Set结构本身。
delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
has(value)：返回一个布尔值，表示该值是否为Set的成员。
clear()：清除所有成员，没有返回值
```js
s.add(1).add(2).add(2);// 注意2被加入了两次
s.size // 2
s.has(1) // true
s.has(2) // true
s.has(3) // false
s.delete(2);
s.has(2) // false
```

#### Array.from方法可以将Set结构转为数组。
```js
var items = new Set([1, 2, 3, 4, 5]);
var array = Array.from(items);

let set = new Set(['red', 'green', 'blue']);
let arr = [...set];
// ['red', 'green', 'blue']
```
#### 这就提供了去除数组重复成员的另一种方法。
```js
function dedupe(array) {
  return Array.from(new Set(array));
}

dedupe([1, 1, 2, 3]) // [1, 2, 3]

let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];
// [3, 5, 2]
```

#### Set结构的实例有四个遍历方法，可以用于遍历成员。
keys()：返回键名的遍历器
values()：返回键值的遍历器
entries()：返回键值对的遍历器
forEach()：使用回调函数遍历每个成员
keys方法、values方法、entries方法返回的都是遍历器对象（详见《Iterator 对象》一章）。由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以keys方法和values方法的行为完全一致。
```js
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
for (let item of set {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```

#### forEach()
Set结构的实例的forEach方法，用于对每个成员执行某种操作，没有返回值。

#### 数组的map和filter方法也可以用于Set了。
```js
let set = new Set([1, 2, 3]);
set = new Set([...set].map(x => x * 2));
// 返回Set结构：{2, 4, 6}

let set = new Set([1, 2, 3, 4, 5]);
set = new Set([...set].filter(x => (x % 2) == 0));
// 返回Set结构：{2, 4}
```
#### Set可以很容易地实现并集（Union）、交集（Intersect）和差集（Difference）
```js
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// 差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```

### Proxy
Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。
ES6 原生提供 Proxy 构造函数，用来生成 Proxy 实例。
```js
var proxy = new Proxy(target, handler);

let obj = {
  name:'12',
  age:12
}
let validator = {
  set: function(obj, prop, value) {
    if (prop === 'age') {
      if (!Number.isInteger(value)) {
        throw new TypeError('The age is not an integer');
      }
      if (value > 200) {
        throw new RangeError('The age seems invalid');
      }
    }

    // 对于age以外的属性，直接保存
    obj[prop] = value;
  }
};
let person = new Proxy({}, validator);
person.age = 100;
person.age // 100
person.age = 'young' // 报错
person.age = 300 // 报错
```
Proxy 对象的所有用法，new Proxy() 表示生成一个Proxy实例，
target参数表示所要拦截的目标对象，
handler参数也是一个对象，用来定制拦截行为。

### Promise
采用链式的then，可以指定一组按照次序调用的回调函数。这时，前一个回调函数，有可能返回的还是一个Promise对象（即有异步操作），这时后一个回调函数，就会等待该Promise对象的状态发生变化，才会被调用。
```js
getJSON("/post/1.json").then(function(post) {
  return getJSON(post.commentURL); // 返回的还是一个Promise
}).then(function funcA(comments) {
  console.log("Resolved: ", comments);
}, function funcB(err){
  console.log("Rejected: ", err);
});
```
Promise.all方法用于将多个Promise实例，包装成一个新的Promise实例。
romise.race方法同样是将多个Promise实例，包装成一个新的Promise实例。

