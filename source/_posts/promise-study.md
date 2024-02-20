---
title: promise
date: 2023-11-10 10:36:05
tags:
categories: js
---

# promise

## 一、基本用法

> Promise 实例生成以后，可以用 then 方法分别指定 resolved 状态和 rejected 状态的回调函数。

```
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  });
}

timeout(100).then((value) => {
  console.log(value);
});
```

> Promise 新建后就会立即执行。

> resolve 函数的参数除了正常值以外，还可能是另一个 Promise 示例，那么当前 promise 的状态就需要等待 resolve 中的 promise 的状态改变。例如

p2 需要等待 p1 的状态改变才能执行它的回调函数

```
const p1 = new Promise(function (resolve, reject) {
  setTimeout(() => reject(new Error('fail')), 3000)
})

const p2 = new Promise(function (resolve, reject) {
  setTimeout(() => resolve(p1), 1000)
})

p2
  .then(result => console.log(result))
  .catch(error => console.log(error))
```

> 调用 resolve 或 reject 并不会终结 Promise 的参数函数的执行

## promise.prototype.then()

then 方法的第一个参数是 resolved 状态的回调函数，第二个参数是 rejected 状态的回调函数，它们都是可选的。

> then 方法返回的是一个新的 Promise 实例，所以可以采用链式调用的方案

## promise.prototype.catch()

用于指定发生错误时的回调函数。

## Promise.prototype.finally()

指定不管 Promise 对象最后状态如何，都会执行的操作

## Promise.all()

Promise.all()方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

所有的请求都完成了，才算完成；如果有一个没完成，状态就变成 rejected

## Promise.race()

```
const p = Promise.race([p1, p2, p3]);
```

只要有一个完成就完成，返回完成最快的一个

## Promise.any()

Promise.any()跟 Promise.race()方法很像，只有一点不同，就是 Promise.any()不会因为某个 Promise 变成 rejected 状态而结束，必须等到所有参数 Promise 变成 rejected 状态才会结束。

## Promise.resolve()

现有对象转为 Promise 对象

## Promise.reject()

返回一个新的 Promise 实例，该实例的状态为 rejected。
