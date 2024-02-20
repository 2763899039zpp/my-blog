---
title: vue3 中如何使用 ref 获取 元素节点
date: 2023-04-17 11:22:31
tags: [js, vue]
categories: vue
---

## vue3 中使用 ref 访问元素

```js

<script>
<script setup>
import { ref, onMounted } from 'vue'

// 声明一个 ref 来存放该元素的引用
// 必须和模板里的 ref 同名
const input = ref(null)

onMounted(() => {
  input.value.focus()
})
</script>

<template>
  <input ref="input" />
</template>
```

这里需要注意的是，你只可以在**组件挂载后**才能访问模板引用

如果你需要侦听一个模板引用 ref 的变化，确保考虑到其值为 null 的情况：

```js
watchEffect(() => {
  if (input.value) {
    input.value.focus();
  } else {
    // 此时还未挂载，或此元素已经被卸载（例如通过 v-if 控制）
  }
});
```

## v-for 中的模板引用

当在 v-for 中使用模板引用时，对应的 ref 中包含的值是一个数组，它将在元素被挂载后包含对应整个列表的所有元素：

```js
<script setup>
import { ref, onMounted } from 'vue'

const list = ref([
  /* ... */
])

const itemRefs = ref([])

onMounted(() => console.log(itemRefs.value))
</script>

<template>
  <ul>
    <li v-for="item in list" ref="itemRe fs">
      {{ item }}
    </li>
  </ul>
</template>
```

应该注意的是，ref 数组并不保证与源数组相同的顺序。

## ref 绑定函数

{% asset_img img-1.jpg 官网的描述 %}

单个元素使用

```js
<template>
  <div :ref="setHelloRef">hello</div>
</template>
<script setup lang="ts">
import { ComponentPublicInstance, HTMLAttributes } from "vue";

const setHelloRef = (el: HTMLElement | ComponentPublicInstance | HTMLAttributes) => {
  console.log(el); // <div>hello</div>
};
</script>

```

v-for 中使用

```js
<template>
  <ul>
    <li v-for="item in 10" :ref="(el) => setItemRefs(el, item)">
      {{ item }}
    </li>
  </ul>
</template>
<script setup lang="ts">
import { ComponentPublicInstance, HTMLAttributes, onMounted } from "vue";
let itemRefs: Array<any> = [];
const setItemRefs = (el: HTMLElement | ComponentPublicInstance | HTMLAttributes, item:number) => {
  if(el) {
    itemRefs.push({
      id: item,
      el,
    });
  }
}
onMounted(() => {
  console.log(itemRefs);
});
</script>
```

输出结果

{% asset_img img-2.jpg 输出结果 %}

在 v-for 中使用函数的方式传入 ref，为了区别出 ref 是哪一个 li，我们将`item`传入函数，这样获得的`itemRefs`的可操作性更强，还解决了 v-for 循环是 ref 数组与原数组顺序不对应的问题。
