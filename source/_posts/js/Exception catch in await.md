---
title: async的用法和异常捕获
date: 2023-03-21 15:45:22
tags:
categories: js
---

### 基本用法

async 函数返回一个 Promise 对象，可以使用 then 方法添加回调函数。当函数执行的时候，一旦遇到 await 就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

```js
async function getStockPriceByName(name) {
  const symbol = await getStockSymbol(name);
  const stockPrice = await getStockPrice(symbol);
  return stockPrice;
}
// 以下是两种调用getStockPriceByName的方法
// 方法一
getStockPriceByName('goog').then(function (result) {
  console.log(result);
});

// 方法二
async function test()=>{
 const result =  await getStockPriceByName('goog');
  console.log(result);
}
```

下面是另一个例子，指定多少毫秒后输出一个值。

```js
function timeout(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}
// async function timeout(ms) {
//  await new Promise((resolve) => {
//    setTimeout(resolve, ms);
//  });
//}
async function asyncPrint(value, ms) {
  await timeout(ms);
  console.log(value);
}

asyncPrint("hello world", 50);
```

上面代码指定 50 毫秒以后，输出 hello world。

async 函数有多种使用形式。

```js
// 函数声明
async function foo() {}

// 函数表达式
const foo = async function () {};

// 箭头函数
const foo = async () => {};
```

### 语法

async 函数返回一个 Promise 对象

async 函数内部 return 语句返回的值，会成为 then 方法回调函数的参数。

```js
async function test() {
  return "hello world";
}

test().then((v) => console.log(v));
// "hello world"
```

async 函数内部抛出错误，会导致返回的 Promise 对象变为 reject 状态。抛出的错误对象会被 catch 方法回调函数接收到

```js
async function f() {
  throw new Error("出错了");
}

f().then(
  (v) => console.log("resolve", v),
  (e) => console.log("reject1", e)
);
//reject Error: 出错了
```

### 错误处理

可以像上面的方法中处理错误
防止出错的方法，也是将其放在 try...catch 代码块之中。

```js
async function f() {
  try {
    await new Promise(function (resolve, reject) {
      throw new Error("出错了");
    });
  } catch (e) {}
  return await "hello world";
}
```

### 使用注意点

第一点，运行结果可能是 rejected，所以最好把 await 命令放在 try...catch 代码块中

第二点，多个 await 命令后面的异步操作，如果不存在继发关系，最好让它们同时触发。

```js
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```

第三点，await 命令只能用在 async 函数之中，如果用在普通函数，就会报错。
如果在循环中使用 await 不能使用 forEach 循环，而应该使用 for 循环；

```js
function dbFuc(db) {
  //这里不需要 async
  let docs = [{}, {}, {}];

  // 可能得到错误结果
  docs.forEach(async function (doc) {
    await db.post(doc);
  });
}
```

上面代码可能不会正常工作，原因是这时三个 db.post()操作将是并发执行，也就是同时执行，而不是继发执行。正确的写法是采用 for 循环。

```js
async function dbFuc(db) {
  let docs = [{}, {}, {}];

  for (let doc of docs) {
    await db.post(doc);
  }
}
```
