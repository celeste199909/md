---
title: Promise 源码实现（上）
tags: Promise
---

## Promises/A + 规范阅读

[地址](https://promisesaplus.com/)

## 完成一个基本的Promise

> 实现基本的resolve、reject和then调用

```js
// index.js
const MyPromise = require("./MyPromise")

let promise = new MyPromise((resolve, reject) => {
    // resolve("resolve value")
    // reject("reject reason")
    throw new Error("err")
}).then(value => {
    console.log(value);
},reason => {
    console.log(reason);
})


// let promise = new Promise((resolve, reject) => {
//     // resolve("resolve")
//     // reject("reject")
//     throw new Error("err")
// })

// promise.then(value => {
//     console.log(value);
// }, reason => {
//     console.log(reason);
// })


```



```js
// MyPromise.js
const PENDING = "pending",
      FULFILLED = "fulfilled",
      REJECTED = "rejected";

class MyPromise{

    constructor(executor){

        this.status = PENDING;
        this.value = undefined;
        this.reason = undefined;

        const resolve = (value) => {
            this.status = FULFILLED;
            this.value = value;
            // console.log(value);
            // console.log("status " + this.status);
        }

        const reject = (reason) => {
            this.status = REJECTED;
            this.reason = reason;
            // console.log(reason);
            // console.log("status " + this.status);
        }
        try{
            executor(resolve, reject)
        }catch(e){
            console.log(e);
        }

    }

    then(onFullfilled, onRejected){
        // console.log(this.status);
        if(this.status === FULFILLED){
            onFullfilled(this.value)
        }

        if(this.status === REJECTED){
            onRejected(this.reason)
        }
    }

}

module.exports = MyPromise
```



## 处理Promise中的异步与多次调用问题

> 需要用到订阅发布模式解决异步问题



```js
// index.js
const MyPromise = require("./MyPromise")

let promise = new MyPromise((resolve, reject) => {
    // resolve("resolve value")
    // reject("reject reason")
    // throw new Error("err")
    setTimeout(()=>{
        resolve("resolve value")
    },2000)
})

promise.then(value => {
    console.log(value,1);
},reason => {
    console.log(reason,1);
})

promise.then(value => {
    console.log(value,2);
},reason => {
    console.log(reason,2);
})


// let promise = new Promise((resolve, reject) => {
//     // resolve("resolve")
//     // reject("reject")
//     throw new Error("err")
// })

// promise.then(value => {
//     console.log(value);
// }, reason => {
//     console.log(reason);
// })


```



```js
// MyPromise.js
const PENDING = "pending",
      FULFILLED = "fulfilled",
      REJECTED = "rejected";

class MyPromise{

    constructor(executor){

        this.status = PENDING;
        this.value = undefined;
        this.reason = undefined;

        this.onFullfilledCallbacks = [];
        this.onRejectedCallbacks = [];

        const resolve = (value) => {
            this.status = FULFILLED;
            this.value = value;
            // console.log(value);
            // console.log("status " + this.status);

            this.onFullfilledCallbacks.forEach(fn => fn())
        }

        const reject = (reason) => {
            this.status = REJECTED;
            this.reason = reason;
            // console.log(reason);
            // console.log("status " + this.status);

            this.onRejectedCallbacks.forEach(fn => fn())
        }
        try{
            executor(resolve, reject)
        }catch(e){
            console.log(e);
        }

    }

    then(onFullfilled, onRejected){
        // console.log(this.status);
        if(this.status === FULFILLED){
            onFullfilled(this.value)
        }

        if(this.status === REJECTED){
            onRejected(this.reason)
        }

        if(this.status === PENDING){
            this.onFullfilledCallbacks.push(() =>{ 
                onFullfilled(this.value)
            })

            this.onRejectedCallbacks.push(() => {
                onRejected(this.reason)
            })
        }
    }

}

module.exports = MyPromise
```






