---
title: 红宝书
date: 2023-03-09 16:42:21
tags: [js, book]
categories: book
---

## 3.1 语法

javascript 区分大小写

标识符：第一个字符必须是一个字母、下划线或者美元符号，其他字符可以是字母下划线美元符号或数字

注释：

```js
// 单行注释

/*
 * 这是一个多行块注释
 *
 */
```

严格模式："use strict"

语句：语句以一个分号结尾；条件控制语句建议使用代码块让编码意图更清晰

## 3.2 关键字和保留字

略

## 3.3 变量

ECMAScript 的变量是松散类型的，所谓松散类型就是可以用来保存任何类型的数据。

```js
var message = "hi";
message = 100; // 有效，但不推荐
```

## 3.4 数据类型

5 种基本的数据类型：Undefined、Null、Boolean、Number、String
1 中简单的复杂类型 Object

### typeof

| 结果（string） |                                                      |
| -------------- | ---------------------------------------------------- |
| "undefined"    | <font color='#CD6600'>如果这个值未定义</font>        |
| "boolean"      | 如果这个值是布尔值                                   |
| "string"       | 如果这个值是字符串                                   |
| "number"       | 如果这个值是数值                                     |
| "object"       | 如果这个值是对象或 <font color='#CD6600'>null</font> |
| "function"     | 如果这个值是函数                                     |

### Undefined 类型

Undefined 类型只有一个值，即特殊的 undefined
声明变量但是未对其初始化时，这个变量的值就是 undefined

注意：对未初始化的变量执行 typeof 操作符会返回 undefined 值，而对未声明的变量执行 typeof 操作符同样也会返回 undefined 值。

但是未初始化的变量执行 typeof 也会返回 undefined 值

```js
var message; // 声明了变量但没有赋值 默认为undefined

// 下面这个变量并没有声明
// var age

alert(typeof message); // "undefined"
alert(typeof age); // "undefined"
```

### Null 类型

从逻辑角度来看，null 值表示一个空对象指针
实际上，undefined 值是派生自 null 值的

```js
var car = null;
alert(typeof car); // "object"

alert(null == undefined); //true
```

### Boolean 类型

该类型只有两个字面值：true 和 false。

转型函数 Boolean() 可将值转化为 Boolean 值
|数据类型|转换为 true 的值|转换为 false 的值|
|--|--|--|
|Boolean |true |false |
|String| 任何非空字符串| ""（空字符串）
|Number| 任何非零数字值（包括无穷大）| 0 和 NaN
|Object| 任何对象| null
|Undefined| n/a |undefined

### Number 类型

不同的数值字面量格式 ；八进制字面值的第一位必须是零（0）；十六进制前两位必须是 0x

```js
var intNum = 55; // 整数 十进制
var octalNum1 = 070; // 八进制的 56
var hexNum1 = 0xa; // 十六进制的 10
```

#### 浮点数值

如果浮点数值本身表示的就是一个整数（如 1.0），那么该值会被转换为整数，节省内存空间

```js
var floatNum1 = 1.1;
var floatNum2 = 10.0; // 整数——解析为 10
```

对于那些极大或极小的数值，可以用 e 表示法（即科学计数法）表示的浮点数值表示

ECMASctipt 会将那些小数点后面带有 6 个零以上的浮点数值转换为以 e 表示法表示的数值

```js
var floatNum = 3.125e7; // 等于 31250000

var a = 0.0000003; // 会被转换成 3e-7
```

#### 数值范围

`Number.MIN_VALUE` 5e-324

`Number.MAX_VALUE` 1.7976931348623157e+308

超出最大的值会自动转化成 Infinity 负数转化为-Infinity

`isFinite()`函数，想确定一个数值是不是位于最小和最大的数值之间

#### NaN

NaN，即非数值（Not a Number）是一个特殊的数值

NaN 与任何值都不相等，包括 NaN 本身

`isNaN()`函数：确定这个参数是否“不是数值”

任何不能被转换为数值的值都会导致这个函数返回 true。

```js
alert(isNaN(NaN)); //true
alert(isNaN(10)); //false（10 是一个数值）
alert(isNaN("10")); //false（可以被转换成数值 10）
alert(isNaN("blue")); //true（不能转换成数值）
alert(isNaN(true)); //false（可以被转换成数值 1）
```

字符串`"blue"`不能被转换成数值，因此函数返回了 true。由于 Boolean 值 true 可以转换成数值 1，因此函数返回 false。

#### 数值转换

Number()

1. 如果是 Boolean 值，true 和 false 将分别被转换为 1 和 0。
2. 如果是数字值，只是简单的传入和返回。
3. 如果是 null 值，返回 0。
4. <font color='#CD6600'>如果是 undefined，返回 NaN</font>。
5. 如果是字符串，遵循下列规则

   1. 是数字就转化成 Number 类型的数字，
   2. 如果是 16 进制的字符串转化为相同大小的十进制整
   3. 如果字符串是空的（不包含任何字符），则将其转换为 0
   4. 其他转化为 NaN 数值；

```js
var num1 = Number("Hello world!"); //NaN
var num2 = Number(""); //0
var num3 = Number("000011"); //11
var num4 = Number(true); //1
```

parseInt()

1. 它会忽略字符串前面的空格，直至找到第一个非空格字符。如果第一个字符不是数字字符或者负号，parseInt()就会返回 NaN；
2. <font color='#CD6600'>parseInt()转换空字符串会返回 NaN</font>

```js
var num1 = parseInt("1234blue"); // 1234
var num2 = parseInt(""); // NaN
var num3 = parseInt("0xA"); // 10（十六进制数）
var num4 = parseInt(22.5); // 22
var num5 = parseInt("070"); // 56（八进制数）
var num6 = parseInt("70"); // 70（十进制数）
var num7 = parseInt("0xf"); // 15（十六进制数）
```

3. parseInt() 的第二个参数指定转化的基数 即按照某一基数转化

```js
var num1 = parseInt("10", 2); //2 （按二进制解析）
var num2 = parseInt("10", 8); //8 （按八进制解析）
var num3 = parseInt("10", 10); //10 （按十进制解析）
var num4 = parseInt("10", 16); //16 （按十六进制解析）
```

parseFloat()

1. parseFloat()也是从第一个字符（位置 0）开始解析每个字符。而且也是一直解析到字符串末尾，或者解析到遇见一个无效的浮点数字字符为止。

2. parseFloat()只解析十进制值

```js
var num1 = parseFloat("1234blue"); //1234 （整数）
var num2 = parseFloat("0xA"); //0
var num3 = parseFloat("22.5"); //22.5
var num4 = parseFloat("22.34.5"); //22.34
var num5 = parseFloat("0908.5"); //908.5
var num6 = parseFloat("3.125e7"); //31250000
```

### String 类型

#### 1.字符字面量

String 数据类型包含一些特殊的字符字面量，也叫转义序列，用于表示非打印字符

#### 2.字符串的特点

ECMAScript 中的字符串是不可变的，也就是说，字符串一旦创建，它们的值就不能改变。要改变某个变量保存的字符串，首先要**销毁**原来的字符串

#### 3.转换为字符串

toString()方法

1. <font color='#CD6600'>null 和 undefined 值没有这个方法</font>。
2. toString()可以输出以二进制、八进制、十六进制，乃至其他任意有效进制格式表示的字符串值。

```js
var num = 10;
alert(num.toString()); // "10"
alert(num.toString(2)); // "1010"
alert(num.toString(8)); // "12"
alert(num.toString(10)); // "10"
alert(num.toString(16)); // "a"
```

String() 方法

1. 如果值有 toString()方法，则调用该方法（没有参数）并返回相应的结果；
2. 如果值是 null，则返回"null"；
3. 如果值是 undefined，则返回"undefined"。

### Object 类型

Object 的每个实例都具有下列属性和方法

1. constructor：保存着用于创建当前对象的函数。对于前面的例子而言，构造函数（constructor）就是 Object()。
2. hasOwnProperty(propertyName)：用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。其中，作为参数的属性名（propertyName）必须以字符串形式指定（例如：o.hasOwnProperty("name")）。
3. isPrototypeOf(object)：用于检查传入的对象是否是传入对象的原型（第 5 章将讨论原型）。
4. propertyIsEnumerable(propertyName)：用于检查给定的属性是否能够使用 for-in 语句（本章后面将会讨论）来枚举。与 hasOwnProperty()方法一样，作为参数的属性名必须以字符
   串形式指定。
5. toLocaleString()：返回对象的字符串表示，该字符串与执行环境的地区对应。
6. toString()：返回对象的字符串表示。
7. valueOf()：返回对象的字符串、数值或布尔值表示。通常与 toString()方法的返回值相同。
