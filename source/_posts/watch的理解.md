---
uuid: 437f4dbd-2f5e-61ff-d790-e58ec08dc7ac
title: watch的理解
date: 2020-04-23 09:57:31
tags: vue
category: vue
---

watch在vue的文档中是这样解释的。
一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。Vue 实例将会在实例化时调用 $watch()，遍历 watch 对象的每一个属性。

值是包括选项的对象：选项包括有三个。
第一个handler：其值是一个回调函数。即监听到变化时应该执行的函数。（内可以监听watch的变化如下：）
第二个是deep：其值是true或false；确认是否深入监听。（一般监听时是不能监听到对象属性值的变化的，数组的值变化可以听到。）
第三个是immediate：其值是true或false；确认是否以当前的初始值执行handler的函数（进入页面就开始 监听）。

$watch可以观察 Vue 实例变化的一个表达式或计算属性函数：
1.监听属性的变化，两个参数，参数1：要监听的对象;
参数2：监听的函数，函数中有两个参数，变化后的新值，变化之前的旧值

```html
<!DOCTYPE html>

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description"
        content="Site Logo Script- Click here for a DHTML script that creates a static logo image, positioned in the lower right corner of the browser." />
    <meta name="keywords"
        content="DHTML, Geocities watermark logo, logo script, static image, DHTML tutorial, free, JavaScript" />
    <title>Dynamic Drive DHTML Scripts- Featured Image Zoomer</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>

<body>
    <div id="app">
        <div>
            <p>changeNum: {{changeNum}}</p>
            <p>watchNum: <input type="text" v-model="watchNum"></p>
            还可以这么写
            <input v-model="example1" />
        </div>
    </div>

    <script>
        let app = new Vue({
            el: "#app",
            data: {
                watchNum: '张三',
                otherNum: ' / ',
                changeNum: ' ',
                example1: ' ',
            },
            watch: {
                watchNum: {
                    handler(newVal, oldVal) {
                        this.changeNum = this.watchNum + this.otherNum + newVal + '---' + oldVal;
                        console.log(this.changeNum)
                        //全部的值 张三 / 张三 ---undefined -- 输入（李四）改变的时候值：张三李四 / 张三李四 ---张三

                        console.log(this.watchNum)
                        //张三 -- 改变的时候：张三李四

                        console.log(this.otherNum)
                        // "/"  改变的收 / 

                        console.log(newVal)
                        //张三 改变的时候：张三李四

                        console.log(oldVal)
                        //undefined 改变的时候：张三

                    },
                    immediate: true,//页面监听开始的时候就直接调用：见上文解释
                    deep: true,//见上文解释
                }
            },
            example1: "exampleMethods",
            methods: {
                exampleMethods(newVal, oldVal) {
                    conosle.log(newVal, oldVal)
                }
            }
        })
    </script>
</body>

</html>
```
