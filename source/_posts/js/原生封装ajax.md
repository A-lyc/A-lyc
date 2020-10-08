---
title: 原生封装ajax
date: 2020-10-08 09:52:48
tags: 原生ajax
category: 原生ajax
---
原生封装js 请求方式 原生，封装的Promise的方法，使用async/await 用try/catch捕捉错误
<!-- more -->
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>封装ajax</title>
</head>
<body>


<script>
    // setTimeout
    // readFile
    // wirteFile
    // readdri
    // ajax
    // 往往一个一部api都会有一个回调函数
    // 原生的
    // 需要建立db.json 来测试，这个可以更换成url地址
    // 原生的方法 callback进行回调
    function get(url, callback) {
        let oReq = new XMLHttpRequest()
        oReq.onload = () => {
            callback(oReq)
        }
        oReq.open('get', url, true)
        oReq.send()
    }

    // 封装的Promise的方法
    function getPromise(url) {
        return new Promise((res, rej) => {
            get(url,item =>{
                if(item.status == 200){
                    return  res(item)
                }else {
                    return  rej('请求失败')
                }
            })
        })
    }

    // 原生请求
    get('db.json', (res) => {
        console.log(res.responseText)
    })

    //封装成Promise - 需要原生请求的支持
    getPromise('db.json').then(res => {
        console.log(res.responseText)
    }).catch(rej => {
        console.log(rej)
    })


    // 使用async/await 用try/catch捕捉错误
    async function asyncawait () {
        try {
            let {responseText} = await getPromise('db.json')
            console.log(responseText)
        }catch (e) {
            console.log(e)
        }
    }
    asyncawait()

</script>
</body>
</html>
```