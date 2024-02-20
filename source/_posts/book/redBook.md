---
title: redBook
date: 2023-03-30 11:24:12
tags:
categories:
---

## 3.5 操作符

### 一元操作符

1. 递增和递减操作符

```js
// 前置 递增 递减
var age = 29;
var anotherAge = --age + 2;
alert(age); // 输出 28
alert(anotherAge); // 输出 30
```

```js
// 后置 递增 递减
var num1 = 2;
var num2 = 20;
var num3 = num1-- + num2; // 等于 22
var num4 = num1 + num2; // 等于 21
```

这里在计算 num3 时使用了 num1 的原始值（2）完成了加计算，而 num4 则使用了递减后的值 （1）

```js
let b = 1;
let a = b++ + 10;
// 可以理解为
// a = b + 10;
// b = b + 1
```

2. 一元加和减操作符
   一元加操作符以一个加号（+）表示，放在数值前面，对数值不会产生任何影响

在对非数值应用一元加操作符时，该操作符会像 Number()转型函数一样对这个值执行转换。

一元减操作符主要用于表示负数

### 布尔操作符

- 逻辑非
  逻辑非操作符首先会将它的操作数转换为一个布尔值，然后再对其求反。
  undefined、NaN、null、"" 、0 返回 true
- 逻辑与 `&&`
- 逻辑或 `||`

### 乘性操作符

`* \ %`

1. Infinity \* 0 = NaN
2. Infinity / Infinity = NaN
3. Infinity / 0 = Infinity
4. Infinity % 3 = NaN
5. 3 % 0 = NaN
6. Infinity % Infinity = NaN
7. 3 % Infinity = 3
8. 0 % 3 = 0

总结，看似可以得到结果，其实只能得到 NaN

### 加性操作符

- 加
  Infinity + -Infinity = NaN
  1 + "1" = "11"
  1 + undefined = NaN
  "1" + undefined = "1undefined"

- 减
  Infinity - Infinity = NaN
  -Infinity - -Infinity = NaN
  Infinity - -Infinity = Infinity
  -Infinity - Infinity = -Infinity

总结，看似结果可以为 0，其实只能得到 NaN

## 关系操作符

如果一个操作数是数值，则将另一个操作数转化为一个数值，然后执行数值比较。

任何操作数与 NaN 进行比较 结果都是 false

### 相等操作符

相等和不相等 先转化再比较

全等 仅比较不转化

操作数和字符串比较，先将字符串转化为数值

null == undefined

### 逗号操作符

可以在一条语句中执行多个操作

```
var num1 = 1,num2 = 2,num3 = 3
```

用于赋值时，总是返回表达式中的最后一项

```
var num = (1,3,4,7); // num = 7
```
