---
title: vue中使用markdown
date: 2023-04-26 14:14:40
tags: vue
categories: vue
---

之前想自己搭建一个个人博客，所以自己用 vue+nodejs 写了一些，但是因为本人的 node 技术有限，项目就做了个基本的增删改查就收工了，因为没有自己的服务器，项目也只能在本地自己玩一玩，所以后来选择使用 hexo 来搭建博客。

这里来讲一下，自己用 vue 搭建博客时使用的到的一个插件`markjs`，写技术文档避免不了需要添加上代码，截图什么的，所以 markdown 来写博客最方便了，不用使用复杂的编辑器插件只需要一个文本输入域就可以搞定。

markjs 主要的功能是将你写的 markdown 字符串转化成 html，你只需要把转换好的 html 展示到页面中即可。

关键语法其实就是`marked.parse();`

{% asset_img demo-1.jpg 页面展示 %}

如图是我自己项目的实现效果

### 安装

```
npm install marked --save
npm install highlight.js --save  // 高亮代码
```

### 使用

```html
<div class="markdown-box">
  <!-- 书写区域 -->
  <textarea
    v-model="ruleForm.content"
    class="markdown-box-write"
    @input="writeChange"
  ></textarea>
  <!-- 编译区域 -->
  <div class="markdown-box-compile markdown-body" v-html="compileHtml"></div>
</div>
```

```js
import { marked } from "marked";
import hljs from "highlight.js";
import "highlight.js/styles/atom-one-dark.css"; // 引入高亮样式 这里我用的是sublime样式


marked.setOptions({
  renderer: new marked.Renderer(),
  // 高亮的语法规范
  highlight: function (code) {
    return hljs.highlightAuto(code).value;
  },
  langPrefix: "hljs language-",
  breaks: false,
  gfm: true,
  headerIds: true,
  headerPrefix: "",
  mangle: true,
  pedantic: false,
  sanitize: false,
  silent: false,
  smartLists: false,
  smartypants: false,
  xhtml: false,
});

const compileHtml = shallowRef();

/** 输入框改变事件 */
const writeChange = (e: Event) => {
  const getTextArea = e.target as HTMLTextAreaElement;
  compileHtml.value = marked.parse(getTextArea.value);
};
```

### 美化样式

通过 mark.js 将 markdown 转化为 html 后因为没有样式 所以会有点丑，这里我使用了`github-markdown-css`来美化样式

- 插件安装

```
npm install github-markdown-css
```

- 在 main.js 引入

```
import 'github-markdown-css/github-markdown.css'
```

- 使用
  在需要使用的容器上添加 class `markdown-body`

```html
<article class="markdown-body">
  <h1>Unicorns</h1>
  <p>All the things</p>
</article>
```

[https://github.com/sindresorhus/github-markdown-css](https://github.com/sindresorhus/github-markdown-css)

### highlight 自定义指令的使用方法

- 引入和定义
  main.js 中

```js
import hljs from 'highlight.js';
import 'highlight.js/styles/atom-one-dark.css'; // 样式 可以自己选择
// 详见官网 https://highlightjs.org/static/demo/

// .....

app.directive('highlight', {
  mounted(el, dir) {
    let blocks = el.querySelectorAll('pre code');
    blocks.forEach((block: any) => {
      hljs.highlightBlock(block);
    });
  },
});
```

- 使用
  在代码块的父标签中使用自定义命令 v-highlight，子元素中的`<pre>`和`<code>`标签中的代码便会自动高亮。

```html
<div v-highlight>
  <pre>
    <code>
function() {
  console.log(\"Hello world!\");
}
    </code>
  </pre>
</div>
```

{% asset_img demo-2.jpg 页面展示 %}
