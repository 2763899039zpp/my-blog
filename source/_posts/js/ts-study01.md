---
title: typescript学习记录
date: 2023-03-27 16:08:09
tags: [ts, js]
categories: js
---

## day01

- 起步

1. 如何初始化一个项目

```
// 新建一个文件夹

npm init -y // 初始化

npm i typescript -g // 全局安装ts

tsc --init //初始化ts配置文件

```

2. 尝试编译 ts 文件
   在项目中创建 src 文件夹，并在 src 文件夹下创建 index.ts 文件

```
tsc ./src/index.ts  // 将ts文件编译成js
```

{% asset_img ts01.jpg 文件目录结构 %}

[https://www.tslang.cn/play/index.html](https://www.tslang.cn/play/index.html)
也可以使用 ts play 来编译我们写好的 ts
{% asset_img ts02.jpg  %}

3. 配置运行环境

```
npm i webpack webpack-cli webpack-dev-server -D

npm i ts-loader typescript -D
# 帮助生成网站首页
npm i html-webpack-plugin -D
# 每次成功构建之后帮助我们清空 dist 目录
npm i clean-webpack-plugin -D
# 合并配置文件
npm i webpack-merge -D
```

新建 build 文件夹，在文件夹中创建配置文件，可参照
[https://github.com/2763899039zpp/ts-study/tree/master/build](https://github.com/2763899039zpp/ts-study/tree/master/build)

配置 package.json

```
# 入口文件
"main": "./src/index.js",


"start": "webpack-dev-server --mode=development --config ./build/webpack.config.js",
"build": "webpack --mode=production --config ./build/webpack.config.js",
```

## 函数

1. 函数定义

以下有定义函数的 4 中方法

```js
function add1(x: number, y: number) {
  return x + y;
}
let add2: (x: number, y: number) => number;

type add3 = (x: number, y: number) => number;

interface add4 {
  (x: number, y: number): number;
}

add2 = (a, b) => {
  return a + b;
};
add2(1, 1);
```

2. 可选参数和默认参数

TypeScript 里的每个函数参数都是必须的，参数多了或少了都会出现错误

可选参数

参数名旁使用 `?`实现可选参数的功能

```js
function buildName(firstName: string, lastName?: string) {
  if (lastName) return firstName + " " + lastName;
  else return firstName;
}

let result1 = buildName("Bob"); // works correctly now
let result2 = buildName("Bob", "Adams", "Sr."); // error, too many parameters
let result3 = buildName("Bob", "Adams"); // ah, just right
```

默认参数

当用户没有传递这个参数或传递的值是 undefined 时，会使用默认参数

```js
function buildName(firstName: string, lastName = "Smith") {
  return firstName + " " + lastName;
}

let result1 = buildName("Bob"); // works correctly now, returns "Bob Smith"
let result2 = buildName("Bob", undefined); // still works, also returns "Bob Smith"
let result3 = buildName("Bob", "Adams", "Sr."); // error, too many parameters
let result4 = buildName("Bob", "Adams"); // ah, just right
```

3. 剩余参数

你想同时操作多个参数，或者你并不知道会有多少参数传递进来,可以使用剩余参数把所有参数收集到一个变量中 `...rest`

```js
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
console.log("employeeName: ", employeeName);
```

4. 函数重载

入参的类型不同时实现不同的功能

```js
function add8(...x: number[]): number
function add8(...x: string[]): string
function add8(...x: any[]): any{
  let first = x[0];
  if (typeof first === 'string') {
    return x.join(",")
  } if (typeof first === 'number') {
    return x.reduce((pre, cur) => pre + cur)
  }
}
console.log(add8('1','2','3')); // 1,2,3
console.log(add8(1,2,3)); // 6
```
