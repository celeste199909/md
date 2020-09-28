---
title: Promise用法（all, race, async await）
tags: Promise
---

## Promise.all()

```js
const fs = require("fs")

function readFile(path, isSetError){
    return new Promise((resolve, reject) => {
        fs.readFile(path, "utf8", (err, data) => {
            if(err || isSetError){
                reject("出错了")
            }
            resolve(data)
        })
    })
}

readFile('./files/1.txt').then(res => {
    console.log(res);
    // return Promise.resolve("读取成功")
    // return Promise.reject("读取失败")
    // 可以返回一个状态，以便在下一个then接收
})
.catch(err => {
    console.log("catch:" + err);
})

// 当reject状态的时候会走catch

// readFile('./files/2.txt').then(res => {
//     console.log(res)
// })

// 1. Promise.all 接收一个interable(可迭代)对象, Array, Set, Map
// 如Array，Array 中可以是Promise 对象，也可以不是
// 如果不是Promise，那么将会被直接Promise.resolve()
// 如果没有元素则返回空数组
// 2. 返回值是一个Promise对象
// Promise.all([
//     readFile('./files/1.txt'),
//     readFile('./files/2.txt', true)
// ]).then(res => {
//     console.log(res);
// }).catch(err => {
//     console.log(err);
// })
// Promise的作用，用来处理多个异步任务并发运行
// 如果其中的一个任务为reject ，那么整个返回的结果就是 reject
// 失败的原因是第一个Promise失败的结果

// Promise.all([
//     false,
//     "a"
// ])
// .then( (res) => {
//     console.log(res);
// })
```

## Promise.race()

```js
// race.js
const fs = require("fs")

function readFile(path, isSetError){
    return new Promise((resolve, reject) => {
        fs.readFile(path, "utf8", (err, data) => {
            if(err || isSetError){
                reject("读取失败");
            }
            resolve(data)
        })
    })
}

Promise.race([
    readFile("./files/1.txt"),
    readFile("./files/2.txt")
]).then(res => {
    console.log(res);
}).catch(err => {
    console.log(err);
})

// Promise.race：哪一个promise最先完成就返回哪个，无论是fullfilled还是rejected
// 如果Promise.race传入空数组则返回 pendding 状态
// 用来测试资源或者接口的响应速度
```

```html
// race.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script>
        function getImage(){
            return new Promise((resolve, reject) => {
                let img = new Image();

                img.onload = function(){
                    resolve(img)
                    document.body.appendChild(img)
                }

                img.src = "https://t9.baidu.com/it/u=2268908537,2815455140&fm=79&app=86&size=h300&n=0&g=4n&f=jpeg?sec=1600066509&t=d39808e89a5ae3aa131814adf64e1455";
            })
        }

        function timeout(){
            return new Promise((resolve, reject) => {
                setTimeout(() => {
                    resolve("请求图片超时")
                },1)
            })
        }

        Promise.race([
            getImage(),
            timeout()
        ])
        .then(res => {
            console.log(res);
        })
        .catch(err => {
            console.log(err);
        })
    </script>
</body>
</html>
```

## async, await

