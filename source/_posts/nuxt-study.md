---
title: nuxt-study
date: 2023-04-14 10:09:02
tags: nuxt
categories:
---

# 概念

## 服务器与浏览器环境

因为你在 Node.js 环境中，你可以访问 Node.js 对象，比如 req 和 res.你不能访问窗口或文档对象，因为它们属于浏览器环境。但是，您可以通过使用 beforeMount 或 mounted 钩子来使用 window 或 document。

## 使用 nuxt 的服务器端呈现步骤

- 步骤 1:浏览器到服务器
  当浏览器发送初始请求时，它将到达 Node.js 内部服务器。next 将生成 HTML 并将其与执行函数的结果一起发送回浏览器，例如 asyncData, nuxtServerInit 或 fetch。钩子函数也被执行。

- 步骤 2:服务器到浏览器
  浏览器从服务器接收呈现的带有生成的 HTML 的页面。内容被显示出来，Vue.js 的水合作用开始发挥作用，使其具有反应性。在此过程之后，页面是交互式的。

- 步骤 3:浏览器到浏览器
  使用`<NuxtLink>`在页面之间导航是在客户端完成的，所以除非硬刷新浏览器，否则不会再次访问服务器。

## context and helper

### context

`context`提供了有关应用程序当前请求的附加且通常是可选的信息。

`context` 对象在特定的 nuxt 函数中可用，如`asyncData`、`plugins`、`middleware`和`nuxtServerInit`。它提供了有关应用程序当前请求的附加且通常是可选的信息。

首先，`context`用于提供对 nuxt 应用程序其他部分的访问，例如 Vuex 存储或底层连接实例。因此，我们在服务器端拥有`context`的 req 和 res 对象，并且存储始终可用。但随着时间的推移，`context`扩展了许多其他有用的变量和快捷方式。现在我们可以在开发模式下访问 HMR(热模块重载/替换)功能、当前路由、页面参数和查询，以及通过`context`访问环境变量的选项。此外，模块函数和 helper 可以通过`context`公开，以便在客户端和服务器端都可用。

### helper

- `$nuxt.isOffline`
  帮助器提供了一种快速查明用户的 internet 连接是否存在的方法:它公开布尔值`isOffline`和`isOnline`。我们可以使用它们在用户脱机(例如)时立即显示消息。

```html
<template>
  <div>
    <div v-if="$nuxt.isOffline">You are offline</div>
    <Nuxt />
  </div>
</template>
```

- `$nuxt.refresh`

当您希望为用户刷新当前页面时，您不希望完全重新加载页面，因为您可能会再次访问服务器，并至少重新初始化整个 next 应用程序。相反，您通常只想刷新由`asyncData`或`fetch`提供的数据。

你可以这样做，通过使用`$nuxt.refresh()`!

- `$nuxt.$loading`

使用`$nuxt`，您还可以通过`$nuxt.$loading`以编程方式控制 nuxt 的加载条。

- `onNuxtReady` helper
  如果希望在加载 nuxt 应用程序并准备好之后运行一些脚本，则可以使用该窗口。onNuxtReady 函数。当您希望在客户端执行一个函数而不增加站点的交互时间时，这一点非常有用。

```js
window.onNuxtReady(() => {
  console.log('Nuxt is ready and mounted');
});
```

- `Process` helpers
  nuxt 将三个布尔值(client, server, 和 static)注入到全局进程对象中，这将帮助您确定应用程序是在服务器上呈现还是完全在客户端上呈现，以及检查静态站点生成。这些帮助程序可以在整个应用程序中使用，并且通常用于`asyncData`用户代码中。

```html
<template>
  <h1>I am rendered on the {{ renderedOn }} side</h1>
</template>

<script>
  export default {
    asyncData() {
      return { renderedOn: process.client ? 'client' : 'server' };
    },
  };
</script>
```

## nuxt 生命周期

{% asset_img lifeLoop.jpg 生命周期%}

然后是 vue 生命周期

beforeCreated&&created 及运行在服务端也运行在客户端

# 特性

## nuxt component

nuxt 附带了一些开箱即用的重要组件，这些组件在构建应用程序时非常有用。这些组件是全局可用的，这意味着您不需要导入它们就可以使用它们。

- `<nuxt>` The Nuxt Component
  > `<nuxt>`组件是用来显示页面组件的组件。基本上，根据所显示的页面，该组件将被页面组件内部的内容所替换。因此，将< next >组件添加到布局中是很重要的。

```html
<template>
  <div>
    <div>My nav bar</div>
    <Nuxt />
    <div>My footer</div>
  </div>
</template>
```

- `<nuxt-child>` 组件
  > 该组件用于显示嵌套路由场景下的页面内容。

<nuxt-child/> 接收 keep-alive 和 keep-alive-props:

```html
<template>
  <div>
    <nuxt-child keep-alive :keep-alive-props="{ exclude: ['modal'] }" />
  </div>
</template>

<!-- 将被转换成以下形式 -->
<div>
  <keep-alive :exclude="['modal']">
    <router-view />
  </keep-alive>
</div>
```

- `<nuxt-link>` 组件

> nuxt-link 组件用于在页面中添加链接至别的页面。

```html
<template>
  <div>
    <h1>Home page</h1>
    <nuxt-link to="/about">关于</nuxt-link>
  </div>
</template>
```

- The `<no-ssr>` 组件

```html
<template>
  <div>
    <sidebar />
    <no-ssr placeholder="Loading...">
      <!-- 此组件仅在客户端呈现 -->
      <comments />
    </no-ssr>
  </div>
</template>
```

## store

### The nuxtServerInit Action

如果在状态树中指定了 nuxtServerInit 方法，Nuxt.js 调用它的时候会将页面的上下文对象作为第 2 个参数传给它（服务端调用时才会酱紫哟）。当我们想将服务端的一些数据传到客户端时，这个方法是灰常好用的。

主要用于初始化东西到 store 中

```js
actions: {
  nuxtServerInit ({ commit }, { req }) {
    if (req.session.user) {
      commit('user', req.session.user)
    }
  }
}
```

## 路由

### 路由守卫

1. 前置
   依赖中间件 middleware

- 全局守卫
  nuxt.config 指向 middleware
  layouts 定义中间件
- 独享组件守卫
  middleware

- 插件全局守卫

2. 后置
   使用 vue 的 beforeRouteLeave 钩子
   插件全局后置守卫

## vuex

### 状态持久化

使用 cookie-universal-nuxt

安装 cookie-universal-nuxt，同步 vuex&&cookie，强制刷新后去除 cookies，同步 vuex，axios 拦截器读取 vuex

{% asset_img nuxt01.jpg 文件目录结构 %}
