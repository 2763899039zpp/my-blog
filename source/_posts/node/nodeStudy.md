---
title: node 学习（自用）
date: 2023-03-09 16:42:40
tags: [node, js]
categories: node
---

## 2 fs 文件系统模块

```js
const fs = require("fs");
```

### 2.1 读取文件中的指定内容

fs.readFile() 语法格式

```js
fs.readFile(path[,options],callback)
```

path：路径
options：编码格式
callback：读完文件后拿到的结果

```js
const fs = require("fs");
fs.readFile("./files/11.txt", "utf8", function (err, dataStr) {
  if (err) {
    return console.log("读取文件失败！" + err.message);
  }
  console.log("文件读取成功，内容是：" + dataStr);
});
```

### 2.2 fs 向指定文件中写入内容

```
fs.writeFile(file,data[,options]) ,callback
```

file：文件内容
data：写入的内容
options：写入内容的格式
callback：写完文件后拿到的结果

```
fs.writeFile('./files/22.txt', 'Hello Node.js', function (err) {
  if (err) {
    return console.log('文件写入失败！' + err.message)
  }
  console.log('文件写入成功！')
})
```

### 2.3 fs 模块路径动态拼接的问题

在使用 fs 模块操作文件时，如果提供的操作路径是以`./`或`../`开头的相对路径时，很容易出现路径动态拼写错误的问题
原因：代码在运行的 1 时候，会执行 node 命令所处的目录，动态拼接出被操作文件的完整路径。

解决方法：直接提供一个完整的文件存放路径

`__dirname`表示当前目录所处的目录

## 3 path 路径模块

### 3.1 什么是 path 路径模块

path 模块是 Node.js 官方提供的、用来处理路径的模块。他提供了一系列的方法和属性，用来满足用户对路径的处理需求。
path.join()方法，用来将多个路径拼接成一个完整的路径字符串
path.basename()方法，用来从字符串中，解析出文件名

导入

```
const path = require('path')
```

### 3.2 路径拼接

```
const  pathStr = path.join('/a','/b/c','../../','./d','e')
```

注意 ../会抵消前面的路径

路径拼接的时候尽量使用 path.join()方法而不是字符串的拼接方法。

### 3.3 获取文件中的文件名

```
path.basename(path[,ext])
```

path 必选参数，表示路径的字符串
ext 可选参数，表示文件的扩展名
返回 表示路径中的最后一部分

### 3.4 获取路径文件中的文件扩展名

```
path.extname(path)
```

path 必选参数，表示路径字符串
返回 返回得到扩展名字符串

### 3.5 综合案例

注意
fs.writeFile()方法只能用来创建路径
重复调用 fs.writeFile()写入同一个文件，新写入的内容会覆盖之前旧的内容。

## 4 http 模块

### 4.1 什么是 http 模块

在网络节点中，负责消费资源的电脑，叫做客户端；**_负责对外提供网络资源的电脑_**，叫做服务器。

http 模块是 Node.js 官方提供的、用来创建 web 服务器的模块。通过 http 模块提供的 `http.createServer()` 方法，就
能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务。

```
const http = require('http')
```

### 4.2 进一步理解 http 模块的作用

服务器和普通电脑的区别在于，服务器上安装了 web 服务器软件，例如：IIS、Apache 等。通过安装这些服务器软件，
就能把一台普通的电脑变成一台 web 服务器。

在 Node.js 中，我们不需要使用 IIS、Apache 等这些第三方 web 服务器软件。因为我们可以基于 Node.js 提供的
http 模块，通过几行简单的代码，就能轻松的手写一个服务器软件，从而对外提供 web 服务。

### 4.3 服务器相关的概念

1. IP 地址
   IP 地址就是互联网上每台计算机的唯一地址，因此 IP 地址具有唯一性。如果把“个人电脑”比作“一台电话”，那么“IP 地址”就相当于“电话号码”，只有在知道对方 IP 地址的前提下，才能与对应的电脑之间进行数据通信。
   IP 地址的格式：通常用“点分十进制”表示成（a.b.c.d）的形式，其中，a,b,c,d 都是 0~255 之间的十进制整数。例如：用点分十进表示的 IP 地址（192.168.1.1）
   注意：
   ① <font color=red>互联网中每台 Web 服务器，都有自己的 IP 地址</font>，例如：大家可以在 Windows 的终端中运行 ping www.baidu.com 命
   令，即可查看到百度服务器的 IP 地址。
   ② 在开发期间，自己的电脑既是一台服务器，也是一个客户端，为了方便测试，可以在自己的浏览器中输入 `127.0.0.1` 这个
   IP 地址，就能把自己的电脑当做一台服务器进行访问了。

2. 域名和域名服务器
   尽管 IP 地址能够唯一地标记网络上的计算机，但 IP 地址是一长串数字，不直观，而且不便于记忆，于是人们又发明了另一套
   字符型的地址方案，即所谓的<font color=red>域名（Domain Name）地址</font>。
   IP 地址和域名是一一对应的关系，这份对应关系存放在一种叫做域名服务器(DNS，Domain name server)的电脑中。使用者
   只需通过好记的域名访问对应的服务器即可，对应的转换工作由域名服务器实现。因此，<font color=red>域名服务器就是提供 IP 地址和域名之间的转换服务的服务器</font>。
   注意：
   ① 单纯使用 IP 地址，互联网中的电脑也能够正常工作。但是有了域名的加持，能让互联网的世界变得更加方便。
   ② 在开发测试期间， `127.0.0.1` 对应的域名是 `localhost`，它们都代表我们自己的这台电脑，在使用效果上没有任何区别。
3. 端口号
   计算机中的端口号，就好像是现实生活中的门牌号一样。通过门牌号，外卖小哥可以在整栋大楼众多的房间中，准确把外卖送到你的手中。
   同样的道理，在一台电脑中，可以运行成百上千个 web 服务。每个 web 服务都对应一个唯一的端口号。客户端发送过来的网络请求，通过端口号，可以被准确地交给对应的 web 服务进行处理。
   注意：
   ① 每个端口号不能同时被多个 web 服务占用。
   ② 在实际应用中，URL 中的<font color=red>80 端口可以被省略</font>。

### 4.4 创建最基本的 web 服务器

```js
const http = require("http");
const server = http.createServer();

server.on("request", (req, res) => {
  const url = req.url;
  const method = req.method;
  const str = `your request url is ${url},and request method is ${method}`;
  console.log(str);
  console.log("Someone visit our web server");
  res.end(str);
});
server.listen(8081, () => {
  console.log("serve running at http://127.0.0.1:8081");
});
```

解决中文乱码问题

```
  res.setHeader('Content-Type', 'text/html; charset=utf-8')
```

4.6 案例-实现读取文件的内容并响应到客户端

```js
const http = require("http");

const fs = require("fs");

const path = require("path");

const server = http.createServer();
server.on("request", (req, res) => {
  // 获取到客户端请求的url
  let url = req.url;
  // 把请求的url地址映射到具体的地址
  const fPath = path.join(__dirname, url);

  // 根据映射的文件路径读取文件内容
  fs.readFile(fPath, "utf8", (err, dataStr) => {
    // 文件读取失败，响应错误信息
    if (err) return res.end("404 not found");

    // 文件读取成功，将读取成功的内容响应到客户端
    res.end(dataStr);
  });
});
server.listen(8081, () => {
  console.log("http://127.0.0.1:8081");
});
```

# 模块化

## 1. 模块化的基本概念

模块化是指解决一个复杂问题时，自顶向下逐层<font color=red>把系统划分成若干模块的过程</font>。对于整个系统来说，<font color=skyblue>模块是可组合、分解和更换的单元</font>。

编程领域中的模块化，就是遵守固定的规则，把一个大文件拆成独立并互相依赖的多个小模块。把代码进行模块化拆分的好处：
① 提高了代码的复用性
② 提高了代码的可维护性
③ 可以实现按需加载\

1.2 模块化规范
<font color=red>模块化规范</font>就是对代码进行模块化的拆分与组合时，需要遵守的那些规则。

模块化规范的好处：大家都遵守同样的模块化规范写代码，降低了沟通的成本，极大方便了各个模块之间的相互调用，
利人利己

## 2. Node.js 中的模块化

2.1 Node.js 中模块的分类
Node.js 中根据模块来源的不同，将模块分为了 3 大类，分别是：

- <font color=red>内置模块</font>（内置模块是由 Node.js 官方提供的，例如 fs、path、http 等）
- <font color=red>自定义模块</font>（用户创建的每个 .js 文件，都是自定义模块）
- <font color=red>第三方模块</font>（由第三方开发出来的模块，并非官方提供的内置模块，也不是用户创建的自定义模块，使用前需要先下载）

  2.2 加载模块
  使用`require()`方法，可以加在以上三种模块
  <font color=red>注意：</font>使用 require() 方法加载其它模块时，会执行被加载模块中的代码。

  2.3 Node.js 中的模块作用域
  和函数作用域类似，在自定义模块中定义的变量、方法等成员，<font color=red>只能在当前模块内被访问</font>，这种模块级别的访问限制，叫做模块作用域

  2.4 向外共享模块作用域中的成员

1. module 对象
   在每一个.js 自定义模块中都有一个 module 对象，它存储了当前模块有关的信息，打印如下。
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/644c49dcd1c345dbb5b74009ef5d6a71.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6ZmM5LiK54Of6Zuo5a-S,size_20,color_FFFFFF,t_70,g_se,x_16)
2. module.exports 对象
   在自定义模块中可以使用 module.exports 对象，将模块内部成员共享出去，工外界使用。外界用 require()方法导入自定义模块时，得到的就是 module.exports 所指向的对象。

3. 共享成员时的注意点
   使用 require()方法导入模块时，导入的结果永远以`module.exports`指向的对象为主

```
exports.username = 'zs',
module.exports = {
	gender: '男',
	age: 22
}

// {gender: '男',age: 22}
```

```
module.exports.username = 'zs',
exports = {
	gender: '男',
	age: 22
}

// {username : 'zs'}
```

```
exports.username = 'zs',
module.exports. gender =  ''男
// {username : 'zs' , gender: '男'}
```

```

exports = {
	gender: '男',
	age: 22
}
module.exports= exports
// {username : 'zs' , gender: '男‘’, age: 22}
```

4. exports 对象
   exportd 和 module.exports 指向同一个对象
   由于 module.exports 单词写起来比较复杂，为了简化向外共享成员的代码，Node 提供了 exports 对象。默认情况下，exports 和 module.exports 指向同一个对象。最终共享的结果，还是以 module.exports 指向的对象为准

2.5 Node.js 中的模块化规范

Node.js 遵循了 `CommonJS` 模块化规范，CommonJS 规定了模块的特性和各模块之间如何相互依赖。
CommonJS 规定：
① 每个模块内部，`module` 变量代表当前模块。
② module 变量是一个对象，它的 exports 属性（即 `module.exports`）对外的接口。
③ 加载某个模块，其实是加载该模块的 module.exports 属性。require() 方法用于加载模块。

## 3. npm 与包

3.1 包

1. 什么是包
   Node.js 中的第三方模块又叫做包。
   就像电脑和计算机指的是相同的东西，第三方模块和包指的是同一个概念，只不过叫法不同。
2. 包的来源
   不同于 Node.js 中的内置模块与自定义模块，包是由第三方个人或团队开发出来的，免费供所有人使用。
   注意：Node.js 中的包都是免费且开源的，不需要付费即可免费下载使用
3. 为什么需要包
   由于 Node.js 的内置模块仅提供了一些底层的 API，导致在基于内置模块进行项目开发的时，效率很低。
   <font color=red>包是基于内置模块封装出来的</font>，提供了更高级、更方便的 API，极大的提高了开发效率。 包和内置模块之间的关系，类似于 jQuery 和 浏览器内置 API 之间的关系

- 从 https://www.npmjs.com/ 网站上搜索自己所需要的包
- 从 https://registry.npmjs.org/ 服务器上下载自己需要的包

  3.3 包管理配置文件

3. 快速创建 package.json
   npm 包管理工具提供了一个快捷命令，可以在执行命令时所处的目录中，快速创建 package.json 这个包管理

```
npm init -y
```

① 上述命令只能在英文的目录下成功运行！所以，项目文件夹的名称一定要使用英文命名，不要使用中文，不能出现空格。
② 运行 npm install 命令安装包的时候，npm 包管理工具会自动把包的名称和版本号，记录到 package.json 中。

4. dependencies
   dependencies 专门用来记录您使用 npm install 命令安装了哪些包。
5. devDependencies 节点
   如果某些包只在项目开发阶段会用到，在项目上线之后不会用到，则建议把这些包记录到 devDependencies 节点中。
   与之对应的，如果某些包在开发和项目上线之后都需要用到，则建议把这些包记录到 ddependencies 节点中

```
// 将包记录到devDdependencies节点中
// 简写
npm i 包名 -D
// 完整写法
npm install 包名 --save-dev
```

3.4 解决下包速度慢的问题

利用镜像

```
// 查看当前的下包镜像
npm config get registry
// 将下包的镜像源切换为淘宝镜像源
npm config set registry=https://registry.npm.taobao.org/
```

nrm
为了更方便的切换下包的镜像源，我们可以安装 nrm 这个小工具，利用 nrm 提供的终端命令，可以快速查看和切换下包的镜像源。

```
// 安装nrm
nrm i nrm -g
// 查看所有可用的镜像
nrm ls
//将下包的镜像源切换成tabboo镜像
nrm use taobao
```

3.8 发布包

1. 注册 npm 账号
   ① 访问 https://www.npmjs.com/ 网站，点击 sign up 按钮，进入注册用户界面
   ② 填写账号相关的信息：Full Name、Public Email、Username、Password
   ③ 点击 Create an Account 按钮，注册账号
   ④ 登录邮箱，点击验证链接，进行账号的验证

2. 登录 npm 账号
   npm 账号注册完成后，可以在终端中执行 npm login 命令，依次输入用户名、密码、邮箱后，即可登录成功。
   <font color=red>注意</font>：在运行 npm login 命令之前，必须
   先把下包的服务器地址切换为 npm 的官方服务器。否则会导致发布包失败

3. 把包发布到 npm 上
   将终端切换到包的根目录之后，运行 npm publish 命令，即可将包发布到 npm 上（注意：包名不能雷同）。

4. 删除已发布的包
   运行 npm unpublish 包名 --force 命令，即可从 npm 删除已发布的包

## 4. 模块的加载机制

4.1 优先从缓存中加载
模块在第一次加载后会被缓存。 这也意味着多次调用 require() 不会导致模块的代码被执行多次。
注意：不论是内置模块、用户自定义模块、还是第三方模块，它们都会优先从缓存中加载，从而提高模块的加载效率

4.2 内置模块的加载机制
内置模块是由 Node.js 官方提供的模块，内置模块的加载优先级最高。

4.3 自定义模块的加载机制
使用 require() 加载自定义模块时，必须指定以 ./ 或 ../ 开头的路径标识符。在加载自定义模块时，如果没有指定 ./ 或 ../ 这样的路径标识符，则 node 会把它当作内置模块或第三方模块进行加载。
同时，在使用 require() 导入自定义模块时，如果省略了文件的扩展名，则 Node.js 会按顺序分别尝试加载以下的文件：
① 按照确切的文件名进行加载
② 补全 .js 扩展名进行加载
③ 补全 .json 扩展名进行加载
④ 补全 .node 扩展名进行加载
⑤ 加载失败，终端报错

4.4 第三方模块的加载机制
如果传递给 require() 的模块标识符不是一个内置模块，也没有以 ‘./’ 或 ‘../’ 开头，则 Node.js 会从当前模块的父目录开始，尝试从 /node_modules 文件夹中加载第三方模块。
如果没有找到对应的第三方模块，则移动到再上一层父目录中，进行加载，直到文件系统的根目录。

4.5 目录作为模块
当把目录作为模块标识符，传递给 require() 进行加载的时候，有三种加载方式：
① 在被加载的目录下查找一个叫做 package.json 的文件，并寻找 main 属性，作为 require() 加载的入口
② 如果目录里没有 package.json 文件，或者 main 入口不存在或无法解析，则 Node.js 将会试图加载目录下的 index.js 文件。
③ 如果以上两步都失败了，则 Node.js 会在终端打印错误消息，报告模块的缺失：Error: Cannot find module 'xxx'

## Express

### 1. 初识 Express

1.1 Express 简介

1. 什么是 Express
   官方给出的概念：Express 是基于 Node.js 平台，快速、开放、极简的 Web 开发框架。
   通俗的理解：Express 的作用和 Node.js 内置的 http 模块类似，<font color=red>是专门用来创建 Web 服务器的</font>。
   Express 的本质：就是一个 npm 上的第三方包，提供了快速创建 Web 服务器的便捷方法

1.2 Express 的基本使用

安装

```
npm i express@4.17.1
```

```js
const express = require("express");
const app = express();

// 通过 app.get() 方法，可以监听客户端的 GET 请求
app.get("/user", (req, res) => {
  res.send({ name: "zs", age: 20, sex: "男" });
});

// 通过 app.post() 方法，可以监听客户端的 POST 请求
app.post("/user", (req, res) => {
  res.send({ name: "ls", age: 20, sex: "男" });
});

// 通过 res.send() 方法，可以把处理好的内容，发送给客户端
app.get("/", (req, res) => {
  // 通过 req.query 对象，可以访问到客户端通过查询字符串的形式，发送到服务器的参数
  // ?name=zs&age=20
  console.log(req.query);
  res.send(req.query);
});

// 通过 req.params 对象，可以访问到 URL 中，通过 : 匹配到的动态参数：
app.get("/user/:id", (req, res) => {
  console.log(req.params);
  res.send(req.params);
});

// 调用express.static() 方法，快速的对外提供静态资源
app.use("/abc", express.static("./clock"));
app.use(express.static("./files"));
// 可以通过 http://127.0.0.1:8081/abc/index.html 访问clock中的文件

app.listen(8081, () => {
  console.log("serve running at http://127.0.0.1:8081");
});
```

### nodemon

1. 为什么要使用 nodemon
   在编写调试 Node.js 项目的时候，如果修改了项目的代码，则需要频繁的手动 close 掉，然后再重新启动，非常繁琐。
   现在，我们可以使用 nodemon（https://www.npmjs.com/package/nodemon） 这个工具，它能够监听项目文件
   的变动，当代码被修改后，nodemon 会自动帮我们重启项目，极大方便了开发和调试

2. 安装 nodemon

```
npm install -g nodemon
```

3. 使用 nodemon

运行 node app.js 命令 ---> nodemon app.js 来启动项目。

```
node app.js
// 改为
nodemon app.js
```

### 2. Express 路由

2.1 路由的概念

1. 什么是路由
   广义上来讲，路由就是映射关系

2. Express 中的路由
   在 Express 中，路由指的是客户端的请求与服务器处理函数之间的映射关系

2.2 路由的使用

1. 最简单的用法

```js
const express = require("express");
const app = express();

// 挂载路由
app.get("/", (req, res) => {
  res.send("hello word");
});

app.post("/", (req, res) => {
  res.send("post request");
});

app.listen(8081, () => {
  console.log("serve running at http://127.0.0.1:8081");
});
```

2. 模块化路由
   ① 创建路由模块对应的 .js 文件
   ② 调用 express.Router() 函数创建路由对象
   ③ 向路由对象上挂载具体的路由
   ④ 使用 module.exports 向外共享路由对象
   ⑤ 使用 app.use() 函数注册路由模块

> 03_router.js

```js
const express = require("express");
const router = express.Router();

router.get("/user/list", (req, res) => {
  res.send("get user list");
});

router.post("/user/add", (req, res) => {
  res.send("Add new user");
});

module.exports = {
  router,
};
```

> 模块化路由.js

```js
const express = require("express");

const app = express();

const { router } = require("./03_router");

// app.use是用来注册全局中间件的  统一加上前缀
app.use("/api", router);

app.listen(8081, () => {
  console.log("serve running at http://127.0.0.1:8081");
});
```

### 3. Express 中间件

3.1 中间件的概念

1. 什么是中间件
   中间件（Middleware ），特指业务流程的中间处理环节
2. 中间件的格式

```
app.get('/',function(req,res,next){
	next();
})
```

3. next 函数的作用
   next 函数是实现多个中间件连续调用的关键，它表示把流转关系转交给下一个中间件或路由。

3.2 中间件初体验 4. 定义中间件函数

```
const mw = function (req, res, next) {
  console.log('中间件')
  next();
}
app.use(mw)
```

5. 定义全局中间件的简化形式

```
// 简化版的中间件
app.use(function (req, res, next) {
  console.log('这是一个最简单的中间件函数')
  next()
})
```

6. 中间件的作用
   多个中间件之间，**共享同一份 req 和 res**。基于这样的特性，我们可以在上游的中间件中，统一为 req 或 res 对象添
   加自定义的属性或方法，供下游的中间件或路由进行使用。
7. 定义多个全局中间件
   可以使用 app.use() 连续定义多个全局中间件。客户端请求到达服务器之后，会按照中间件定义的先后顺序依次进行调用。
8. 局部生效的中间件

```
app.get('/', mw1, (req, res) => {
  res.send('hello word' + req.startTime);
})
app.get('/', mw1, mw2, (req, res) => {
  res.send('hello word' + req.startTime);
})

```

8. 了解中间件的 5 个使用注意事项
   ① 一定要在路由之前注册中间件
   ② 客户端发送过来的请求，可以连续调用多个中间件进行处理
   ③ 执行完中间件的业务代码之后，不要忘记调用 next() 函数
   ④ 为了防止代码逻辑混乱，调用 next() 函数后不要再写额外的代码
   ⑤ 连续调用多个中间件时，多个中间件之间，共享 req 和 res 对象

3.3 中间件的分类

- 应用级别的中间件
- 路由级别的中间件
- 错误级别的中间件
- express 内置中间件
- 第三方中间件

1. 应用级别的中间件
   通过 app.use() 或 app.get() 或 app.post() ，绑定到 app 实例上的中间件，叫做应用级别的中间件
2. 路由级别的中间件
   绑定到 express.Router() 实例上的中间件，叫做路由级别的中间件。它的用法和应用级别中间件没有任何区别。

```
router.use(function(req,res,next){
	console.log('time:',Date.new())
	next();
})
```

3. 错误级别的中间件

```
app.use((err, req, res, next) => {
  console.log('捕获错误')
  res.send(err.message)
  next()
})
```

4. Express 内置的中间件
   ① express.static 快速托管静态资源的内置中间件，例如： HTML 文件、图片、CSS 样式等（无兼容性）
   ② express.json 解析 JSON 格式的请求体数据（有兼容性，仅在 4.16.0+ 版本中可用）
   ③ express.urlencoded 解析 URL-encoded 格式的请求体数据（有兼容性，仅在 4.16.0+ 版本中可用）

```js
app.use(express.json()).use(express.urlencoded({ extended: false }));
```

5. 第三方的中间件
   例如使用 body-parser

```
 npm i body-parser
```

```js
const parser = require("body-parser");

app.use(parser.urlencoded({ extended: false }));
```

6. 自定义解析表单的中间件

```js
const express = require("express");
const qs = require("querystring");
const app = express();
app.use((req, res, next) => {
  // 自定义中间件的具体逻辑
  let str = "";
  req.on("data", (chunk) => {
    str += chunk;
  });
  req.on("end", () => {
    let data = qs.parse(str);
    req.body = data;
    next();
  });
});
app.post("/user", (req, res) => {
  res.send(req.body);
});
app.listen(8081, function () {
  console.log("http://127.0.0.1:8081");
});
```

7. 自定义中间件的模块化
   > 12_custom-body-parser.js

```js
const qs = require("querystring");
const bodyParser = (req, res, next) => {
  let str = "";
  req.on("data", (chunk) => {
    str += chunk;
  });
  req.on("end", () => {
    req.body = qs.parse(str); // 调用qs.prase方法，把查询到的字符串解析为对象
    next();
  });
};
module.exports = bodyParser;
```

```js
const express = require("express");
const qs = require("querystring");
const cors = require("cors");
// 引入中间件
const customBodyParser = require("./12_custom-body-parser");
const app = express();

app.use(cors());
// 使用自定义的中间件
app.use(customBodyParser);
app.post("/user", (req, res) => {
  res.send(req.body);
});
app.listen(8081, function () {
  console.log("http://127.0.0.1:8081");
});
```

### 4. 使用 Express 写接口

4.5 CORS 跨域资源共享

1. 接口的跨域问题
   解决接口跨域问题的方案主要有两种：
   ① CORS（主流的解决方案，推荐使用）
   ② JSONP（有缺陷的解决方案：只支持 GET 请求）

2. 使用 cors 中间件解决跨域问题
   ① 运行 npm install cors 安装中间件
   ② 使用 const cors = require('cors') 导入中间件
   ③ 在路由之前调用 app.use(cors()) 配置中间件

3. CORS 响应头部 - Access-Control-Allow-Origin

```
res.setHeader('Access-Control-Allow-Origin','http://itcast.cn')
```

通配符 \*，表示允许来自任何域的请求，

```
res.setHeader('Access-Control-Allow-Origin','*')
```

4. CORS 响应头部 - Access-Control-Allow-Headers
   默认情况下，CORS 仅支持客户端向服务器发送如下的 9 个请求头：
   Accept、Accept-Language、Content-Language、DPR、Downlink、Save-Data、Viewport-Width、Width 、
   Content-Type （值仅限于 text/plain、multipart/form-data、application/x-www-form-urlencoded 三者之一）
   如果客户端向服务器发送了额外的请求头信息，则需要在服务器端，通过 Access-Control-Allow-Headers 对额外
   的请求头进行声明，否则这次请求会失败！

```
res.setHeader('Access-Control-Allow-Headers','Content-Type,X-Custom-Header')
```

5. CORS 响应头部 - Access-Control-Allow-Methods
   默认情况下，CORS 仅支持客户端发起 GET、POST、HEAD 请求。
   如果客户端希望通过 PUT、DELETE 等方式请求服务器的资源，则需要在服务器端，通过 Access-Control-Alow-Methods
   来指明实际请求所允许使用的 HTTP 方法。

```
// 只允许post get post head 请求
res.setHeader('Access-Control-Allow-Methods','POST','GET','DELETE','HEAD');
// 允许所有的http请求方法
res.setHeader('Access-Control-Allow-Methods','*');
```

6. CORS 请求的分类
   客户端在请求 CORS 接口时，根据请求方式和请求头的不同，可以将 CORS 的请求分为两大类，分别是：
   ① 简单请求
   ② 预检请求

7. 简单请求
   同时满足以下两大条件的请求，就属于简单请求：
   ① 请求方式：GET、POST、HEAD 三者之一
   ② HTTP 头部信息不超过以下几种字段：无自定义头部字段、Accept、Accept-Language、Content-Language、DPR、Downlink、Save-Data、Viewport-Width、Width 、Content-Type（只有三个值 application/x-www-formurlencoded、multipart/form-data、text/plain）

8. 预检请求
   只要符合以下任何一个条件的请求，都需要进行预检请求：
   ① 请求方式为 GET、POST、HEAD 之外的请求 Method 类型
   ② 请求头中包含自定义头部字段
   ③ 向服务器发送了 application/json 格式的数据
   在浏览器与服务器正式通信之前，浏览器会先发送 OPTION 请求进行预检，以获知服务器是否允许该实际请求，所以这一
   次的 OPTION 请求称为“预检请求”。服务器成功响应预检请求后，才会发送真正的请求，并且携带真实数据。

9. 简单请求和预检请求的区别
   简单请求的特点：客户端与服务器之间只会发生一次请求。
   预检请求的特点：客户端与服务器之间会发生两次请求，OPTION 预检请求成功之后，才会发起真正的请求。

# 数据库操作

### 4. 在项目中操作 MySQL

4.1 在项目中操作数据库的步骤
① 安装操作 MySQL 数据库的第三方模块（mysql）
② 通过 mysql 模块连接到 MySQL 数据库
③ 通过 mysql 模块执行 SQL 语句
4.2 安装与配置 mysql 模块

1. 安装

```
npm install mysql
```

2. 配置 mysql 模块

```
const mysql = require('mysql');
const db = mysql.createPool({
  host: '127.0.0.1',
  user: 'root',
  password: '1234',
  database: "student"
})
```

4.3 使用 mysql 模块操作 MySQL 数据库

```
// 查询user表中的数据
db.query('SELECT * from user', (err, result) => {
  if (err) return console.log(err.message);
  console.log(result)
})
```

```
// 向user表中插入一条数据
const user = { name: 'zhangpp', score: 20, sex: '女' };
const sqlStr = 'insert into user (name,score,sex) values (?,?,?)';

db.query(sqlStr, [user.name, user.score, user.sex], (err, result) => {
  if (err) return console.log(err.message);
  if (result.affectedRows == 1) {
    console.log('插入成功')
  }
  console.log(result)
})
```

```
// 插入一行的简单写法
const user = { name: 'zhangpp', score: 20, sex: '女' };
const sqlStr = 'insert into user set ?'
db.query(sqlStr, user, (err, result) => {
  if (err) return console.log(err.message);
  if (result.affectedRows == 1) {
    console.log('插入成功')
  }
  console.log(result)
})
```

```
// 更新数据
const user = { id: 3, name: 'lll', score: 15, sex: '女' };
const sqlStr = 'update user set name=?,score=? where id=?'
db.query(sqlStr, [user.name, user.score, user.id], (err, result) => {
  if (err) return console.log(err.message);
  if (result.affectedRows == 1) {
    console.log('修改成功')
  }
  console.log(result)
})
```

```
// 更新数据的便捷方法
const user = { id: 3, name: 'lle', score: 15, sex: '女' };
const sqlStr = 'update user set ? where id=?'
db.query(sqlStr, [user, user.id], (err, result) => {
  if (err) return console.log(err.message);
  if (result.affectedRows == 1) {
    console.log('修改成功')
  }
  console.log(result)
})
```

```
// 删除数据
const sqlStr = "delete from user where id=?"
db.query(sqlStr, 3, (err, result) => {
  if (err) return console.log(err.message);
  if (result.affectedRows == 1) {
    console.log('删除成功')
  }
  console.log(result)
})
```

### 前后端的身份认证

5.1 Web 开发模式
目前主流的 Web 开发模式有两种，分别是：
① 基于服务端渲染的传统 Web 开发模式
② 基于前后端分离的新型 Web 开发模式

1. 服务端渲染的优缺点
   优点：
   ① 前端耗时少。因为服务器端负责动态生成 HTML 内容，浏览器只需要直接渲染页面即可。尤其是移动端，更省电。
   ② 有利于 SEO。因为服务器端响应的是完整的 HTML 页面内容，所以爬虫更容易爬取获得信息，更有利于 SEO。
   缺点：
   ① 占用服务器端资源。即服务器端完成 HTML 页面内容的拼接，如果请求较多，会对服务器造成一定的访问压力。
   ② 不利于前后端分离，开发效率低。使用服务器端渲染，则无法进行分工合作，尤其对于前端复杂度高的项目，不利于
   项目高效开发

2. 前后端分离的优缺点
   优点：
   ① 开发体验好。前端专注于 UI 页面的开发，后端专注于 api 的开发，且前端有更多的选择性。
   ② 用户体验好。Ajax 技术的广泛应用，极大的提高了用户的体验，可以轻松实现页面的局部刷新。
   ③ 减轻了服务器端的渲染压力。因为页面最终是在每个用户的浏览器中生成的。
   缺点：
   ① 不利于 SEO。因为完整的 HTML 页面需要在客户端动态拼接完成，所以爬虫对无法爬取页面的有效信息。（解决方
   案：利用 Vue、React 等前端框架的 SSR （server side render）技术能够很好的解决 SEO 问题！）

5.2 身份认证 3. 不同开发模式下的身份认证
对于服务端渲染和前后端分离这两种开发模式来说，分别有着不同的身份认证方案：
① 服务端渲染推荐使用 `Session` 认证机制
② 前后端分离推荐使用 `JWT` 认证机制

5.4 在 Express 中使用 Session 认证 4. 安装 express-session 中间件

```
npm i express-session
```

// 配置 Session 中间件

```
const session = require('express-session');
app.use(session({
  secret: 'zpp',
  resave: false,
  saveUninitialized: true
}))
```

可通过 req.session 来访问和使用 session 对象

```
app.post('/api/login', (req, res) => {
  // 判断用户提交的登录信息是否正确
  if (req.body.username !== 'admin' || req.body.password !== '000000') {
    return res.send({ status: 1, msg: '登录失败' })
  }

  // 登录成功后的用户信息，保存到 Session 中
  req.session.username = req.body.username;
  req.session.isLogin = true;
  res.send({ status: 0, msg: '登录成功' })
})
```

可以直接从 `req.session` 对象上获取之前存储的数据

```
// 获取用户姓名的接口
app.get('/api/username', (req, res) => {
  // 从 Session 中获取用户的名称，响应给客户端
  if (req.session.isLogin) {
    res.send({
      status: 0,
      msg: 'success',
      username: req.session.username
    })
  } else {
    res.send({
      status: 1,
      msg: 'fail'
    })
  }
})
```

调用 `req.session.destroy()` 函数，即可清空服务器保存的 session 信息

```
// 退出登录的接口
app.post('/api/logout', (req, res) => {
  // 清空 Session 信息
  req.session.destroy()
  res.send({
    status: 0,
    msg: '退出登录成功'
  })
})
```

5.5 JWT 认证机制

1.  什么是 JWT
    `JWT`（英文全称：JSON Web Token）是目前最流行的**跨域认证解决方案**
2.  JWT 的组成部分
    JWT 通常由三部分组成，分别是 Header（头部）、Payload（有效荷载）、Signature（签名）。
    三者之间使用英文的“.”分隔。
3.  JWT 的三个部分各自代表的含义
    JWT 的三个组成部分，从前到后分别是 Header、Payload、Signature。 - Payload 部分才是真正的用户信息，它是用户信息经过加密之后生成的字符串。 - Header 和 Signature 是安全性相关的部分，只是为了保证 Token 的安全性
4.  JWT 的使用方式 - 客户端收到服务器返回的 JWT 之后，通常会将它储存在 localStorage 或 sessionStorage 中。 - 此后，客户端每次与服务器通信，都要带上这个 JWT 的字符串，从而进行身份认证。
    推荐的做法是把 JWT 放在 HTTP 请求头的 Authorization 字段中，格式如下：

```
Authorization: Bearer <token>
```

5.6 在 Express 中使用 JWT

1. 安装 JWT 相关的包

```
npm install jsontoken express-jwt
```

- jsonwebtoken 用于生成 JWT 字符串
- express-jwt 用于将 JWT 字符串解析还原成 JSON 对象

2. 导入 JWT 相关的包

```
const jwt = require('jsonwebtoken');
const expressJWT = require('express-jwt');
```

3. 定义 secret 密钥

```
const secretKey = 'zhangpp'
```

4. 在登录成功后生成 JWT 字符串
   调用 jsonwebtoken 包提供的 sign() 方法，将用户的信息加密成 JWT 字符串，响应给客户端：

```
 const tokenStr = jwt.sign({
    username: userinfo.username
  }, secretKey, { expiresIn: '60s' })
```

5. 将 JWT 字符串还原为 JSON 对象
   服务器可以通过 express-jwt 这个中间件，自动将客户端发送过来的 Token 解析还原成 JSON 对象

```
app.use(expressJWT({
  secret: secretKey, algorithms: ["HS256"],
  credentialsRequired: false
}).unless({ path: ['/api/login'] }))
```

6. 使用 req.user 获取用户信息

```
app.get('/admin/getinfo', function (req, res) {
  // 使用 req.user 获取用户信息，并使用 data 属性将用户信息发送给客户端
  console.log('admin')
  const user = req.user
  console.log('username: ', user);
  res.send({
    status: 200,
    message: '获取用户信息成功！',
    data: req.user  // 要发送给客户端的用户信息
  })
})
```

7. 捕获解析 JWT 失败后产生的错误

```
app.use((err, req, res, next) => {
  // 这次错误是由token解析失败导致的
  if (err.name === 'UnauthorizedError') {
    return res.send({
      status: 401,
      message: '无效的token',

    })
  } else {
    res.send({
      status: 500,
      message: '未知的错误',
    })
  }
})
```

> 全部代码

```
// 导入 express 模块
const express = require('express')
// 创建 express 的服务器实例
const app = express()

// TODO_01：安装并导入 JWT 相关的两个包，分别是 jsonwebtoken 和 express-jwt
const jwt = require('jsonwebtoken');
const expressJWT = require('express-jwt');

// 允许跨域资源共享
const cors = require('cors')
app.use(cors())

// 解析 post 表单数据的中间件
const bodyParser = require('body-parser')
app.use(bodyParser.urlencoded({ extended: false }))

// TODO_02：定义 secret 密钥，建议将密钥命名为 secretKey
const secretKey = 'zhangpp'
// TODO_04：注册将 JWT 字符串解析还原成 JSON 对象的中间件

app.use(expressJWT({
  secret: secretKey, algorithms: ["HS256"],
  credentialsRequired: false
}).unless({ path: ['/api/login'] }))

// 登录接口
app.post('/api/login', function (req, res) {
  // 将 req.body 请求体中的数据，转存为 userinfo 常量
  const userinfo = req.body
  // 登录失败
  if (userinfo.username !== 'admin' || userinfo.password !== '000000') {
    return res.send({
      status: 400,
      message: '登录失败！'
    })
  }
  // 登录成功
  // TODO_03：在登录成功之后，调用 jwt.sign() 方法生成 JWT 字符串。并通过 token 属性发送给客户端
  const tokenStr = jwt.sign({
    username: userinfo.username
  }, secretKey, { expiresIn: '60s' })

  res.send({
    status: 200,
    message: '登录成功！',
    token: tokenStr // 要发送给客户端的 token 字符串
  })
})

// 这是一个有权限的 API 接口
app.get('/admin/getinfo', function (req, res) {
  // TODO_05：使用 req.user 获取用户信息，并使用 data 属性将用户信息发送给客户端
  console.log('admin')
  const user = req.user
  console.log('username: ', user);
  res.send({
    status: 200,
    message: '获取用户信息成功！',
    data: req.user  // 要发送给客户端的用户信息
  })
})

// TODO_06：使用全局错误处理中间件，捕获解析 JWT 失败后产生的错误
app.use((err, req, res, next) => {
  // 这次错误是由token解析失败导致的
  if (err.name === 'UnauthorizedError') {
    return res.send({
      status: 401,
      message: '无效的token',

    })
  } else {
    res.send({
      status: 500,
      message: '未知的错误',
    })
  }
})

// 调用 app.listen 方法，指定端口号并启动web服务器
app.listen(8081, function () {
  console.log('Express server running at http://127.0.0.1:8081')
})

```
