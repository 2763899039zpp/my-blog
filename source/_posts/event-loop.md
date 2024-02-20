---
title: EventLoop 事件循环
date: 2023-04-24 16:29:07
tags:
categories: js
---

> 前言：不知道是第几次学习 eventLoop 了 每次忘记的时候就回找资料去复习一遍，但是因为自己没有认真总结过，总是很容易忘记，这次就来认真总结一下做到一次搞懂 eventLoop；

经典面试题

```js
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}
async function async2() {
  console.log("async2");
}
console.log("script start");
setTimeout(function () {
  console.log("setTimeout");
}, 0);
async1();
new Promise(function (resolve) {
  console.log("promise1");
  resolve();
}).then(function () {
  console.log("promise2");
});
console.log("script end");
```

看到 `async+setTimeout+Promise`组合的轰炸可能会一时乱了阵脚，那我们接下来就慢慢梳理；

## 常见的宏任务和微任务

- 宏任务：script(整体代码)、setTimeout、setInterval、I/O、事件、postMessage、 MessageChannel、setImmediate (Node.js)
- 微任务：Promise.then、 MutaionObserver、process.nextTick (Node.js)

总结一下，异步任务分为 宏任务(macrotask) 与 微任务 (microtask)。宏任务会进入一个队列，而微任务会进入到另一个不同的队列，且微任务要优于宏任务执行。

```js
setTimeout(() => {
  console.log("A");
}, 0);
var obj = {
  func: function () {
    setTimeout(function () {
      console.log("B");
    }, 0);
    return new Promise(function (resolve) {
      console.log("C");
      resolve();
    });
  },
};
obj.func().then(function () {
  console.log("D");
});
console.log("E");
```

```
输出：C E D A B
```

1. 将 setTimeout 放入宏任务队列 宏任务 ['A']

2. 执行 obj.func()，把 setTimeout 放入宏任务队列 宏任务 ['A','E']

3. 函数返回 Promise ，Promise 中的代码是同步操作，所以先打印 'C'

4. 将 then 后的方法放入微任务队列，微任务['D']

5. 接着执行同步任务 console.log('E');，打印出 'E'

6. 因为微任务优先执行，所以先输出 'D'

7. 最后依次输出 'A' 和 'B'

```js
let p = new Promise((resolve) => {
  resolve(1);
  Promise.resolve().then(() => console.log(2));
  console.log(4);
}).then((t) => console.log(t));
console.log(3);
```

```
输出：4 3 2 1
```

1. 首先将 Promise.resolve() 的 then() 方法放到微任务队列 微任务队列 ['2']

2. 然后打印出同步任务 4

3. 接着将 p 的 then() 方法放到微任务队列 微任务队列['2', '1']

4. 打印出同步任务 3

5. 最后依次打印微任务 2 和 1

```js
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}
async function async2() {
  console.log("async2");
}
console.log("script start");
setTimeout(function () {
  console.log("setTimeout");
}, 0);
async1();
new Promise(function (resolve) {
  console.log("promise1");
  resolve();
}).then(function () {
  console.log("promise2");
});
console.log("script end");
```

```
输出
script start
async1 start
async2
promise1
script end
async1 end
promise2
setTimeout
```
