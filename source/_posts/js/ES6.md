---
title: ES6
date: 2020-08-19 20:14:36
tags: ES6
category: ES6
---
定义变量 let 
定义常量 const
魔法字符串 `${}`
结构语法 let {data} = res;let [a,b,c] = arr
函数默认语法 f function f( x = {}){return x} // 默认空对象
遍历追加数组 arr.push(...array)

throw err -> 阻止程序执行，打印到控制台
<!-- more -->
### 深拷贝，原数组不变
```shell
let target = { a: 1, b: 2 };
let source = { b: 4, c: 5 };
/**
const returnedTarget = Object.assign(target,source);
target
目标对象-将源属性应用到的对象，修改后将返回该对象。

sources
源对象—包含要应用的属性的对象。
*/

const returnedTarget = Object.assign(source);
target.a = 10
console.log(target);
// expected output: Object { a: 10, b: 2 }

console.log(returnedTarget);
// expected output: Object { b: 4, c: 5 };

```

### Number.isFinite()用来检查一个数值是否为有限的（finite），即不是Infinity。
// Number.isFinite(15); 
// trueNumber.isFinite(0.8); 
// trueNumber.isFinite(NaN); 
// falseNumber.isFinite(Infinity); 
// falseNumber.isFinite(-Infinity); 
// falseNumber.isFinite('foo'); 
// falseNumber.isFinite('15'); 
// falseNumber.isFinite(true); 
// false

### Number.isNaN()用来检查一个值是否为NaN。
// Number.isNaN(NaN) 
// trueNumber.isNaN(15) 
// falseNumber.isNaN('15') 
// falseNumber.isNaN(true) 
// falseNumber.isNaN(9/NaN) 
// trueNumber.isNaN('true' / 0) 
// trueNumber.isNaN('true' / 'true') 
// true

### Json传值的几个方法可以试试
1.json.dumps()函数是将字典转化为字符串
import json
dict1 = {"age": "12"}
json_info = json.dumps(dict1)  json_info此时就被转换成一个字符串了
 2.json.loads()函数是将字符串转化为字典
json_info = '{"age": "12"}'
dict1 = json.loads(json_info)  dict1由字符串被转换为一个字典
 3.json.dump()函数的使用，将json信息写进文件
import json
json_msg = {"install": {"install_date": "2018/09/26","install_result": "success"}}
file = open('1.json', 'w')
json.dump(json_msg, file)
 4.json.load()函数的使用，将读取json信息
file = open('1.json','r',encoding='utf-8')
info = json.load(file)
with open('1.json', 'r') as f:
    data = json.load(f)
还是用第二种比较好，不用手动去关文件了

### catch 命令的参数省略
JavaScript 语言的try...catch结构，以前明确要求catch命令后面必须跟参数，接受try代码块抛出的错误对象。
try {
  代码块 - 这里面有错误的时候执行到错误的位置直接跳转到catch这个内，把错误抛出，之后在执行后徐的代码，没有错误的时候全部执行，之后跳过catch这个继续执行下面的程序
  } catch (err) {
  // 处理错误}
  Error.name的六种值对应的信息：
  EvalError: eval()的使用与定义不一致
  RangeError: 数值越界
  ReferenceError: 非法或不能识别的引用数值
  eg：函数未被定义直接调用
  SyntaxError：发生语法解析错误
  eg：出现中文冒号：
  TypeError：操作数类型错误
  URIError: URI处理函数使用不当
上面代码中，catc


### ES6 提供三个新的方法——entries()，keys()和values()——用于遍历数组。它们都返回一个遍历器对象（详见《Iterator》一章），可以用for...of循环进行遍历，唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。
for (let index of ['a', 'b'].keys()) {
  console.log(index);}
// 0// 1for (let elem of ['a', 'b'].values()) {
  console.log(elem);}
// 'a'// 'b'for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);}
// 0 "a"// 1 "b"
如果不使用for...of循环，可以手动调用遍历器对象的next方法，进行遍历。
let letter = ['a', 'b', 'c'];let entries = letter.entries();
console.log(entries.next().value); // [0, 'a']console.log(entries.next().value); // [1, 'b']console.log(entries.next().value); // [2, 'c']
Array.prototype.includes方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似。ES2016 引入了该方法。
[1, 2, 3].includes(2)     // true[1, 2, 3].includes(4)     // false[1, 2, NaN].includes(NaN) // true
该方法的第二个参数表示搜索的起始位置，默认为0。如果第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度（比如第二个参数为-4，但数组长度为3），则会重置为从0开始。


### 另外，Map 和 Set 数据结构有一个has方法，需要注意与includes区分。
Map 结构的has方法，是用来查找键名的，比如Map.prototype.has(key)、WeakMap.prototype.has(key)、Reflect.has(target, propertyKey)。
Set 结构的has方法，是用来查找值的，比如Set.prototype.has(value)、WeakSet.prototype.has(value)
数组的成员有时还是数组，Array.prototype.flat()用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。
[1, 2, [3, 4]].flat()
// [1, 2, 3, 4]
上面代码中，原数组的成员里面有一个数组，flat()方法将子数组的成员取出来，添加在原来的位置。
flat()默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将flat()方法的参数写成一个整数，表示想要拉平的层数，默认为1。
[1, 2, [3, [4, 5]]].flat()
// [1, 2, 3, [4, 5]][1, 2, [3, [4, 5]]].flat(2)
// [1, 2, 3, 4, 5]
flatMap()方法对原数组的每个成员执行一个函数（相当于执行Array.prototype.map()），然后对返回值组成的数组执行flat()方法。该方法返回一个新数组，不改变原数组。展开数组，每个数组执行方法，之后追加到原数组后面形成新的数组
// 相当于 [[2, 4], [3, 6], [4, 8]].flat()
[2, 3, 4].flatMap((x) => [x, x * 2])
// [2, 4, 3, 6, 4, 8]


ES5 对空位的处理，已经很不一致了，大多数情况下会忽略空位。
forEach(), filter(), reduce(), every() 和some()都会跳过空位。
map()会跳过空位，但会保留这个值
join()和toString()会将空位视为undefined，而undefined和null会被处理成空字符串。
