---
title: hexo博客中添加图片并实现点击图片放大的功能
date: 2023-03-28 10:49:55
tags:
categories: hexo
---

## 一、博客中添加图片

可以官网“参考资源文件夹”中的说明[https://hexo.io/zh-cn/docs/asset-folders](https://hexo.io/zh-cn/docs/asset-folders)

> Hexo 将会在你每一次通过 `hexo new [layout] <title>` 命令创建新文章时自动创建一个文件夹。这个资源文件夹将会有与这个文章文件一样的名字。将所有与你的文章有关的资源放在这个关联文件夹中之后，你可以通过相对路径来引用它们，这样你就得到了一个更简单而且方便得多的工作流

1. 打开 资源文件管理功能

```
_config.yml
post_asset_folder: true
```

2. 相对路径引用的标签插件

```
{% asset_img hexo01.jpg test %}
```

{% asset_img hexo01.jpg test %}

## 二、实现图片点击有变大的效果

首先说明：我的博客中使用了 next 主题，该主题提供了图片放大的插件所以就直接使用了

1. 找到该主题所在文件夹，在 `hexo-theme-next/source/lib` 文件夹中 git clone 该插件，参考[https://github.com/theme-next/theme-next-fancybox30](https://github.com/theme-next/theme-next-fancybox3)

```
git clone https://github.com/theme-next/theme-next-fancybox3 fancybox
```

<!-- {% asset_img hexo02.jpg test %} -->

2. 在主题配置文件 `_config.yml`中

```
fancybox: true
```

3. 如果无法访问，可能网络环境的问题们，可以使用 CDN

```
# _config.yml 主题配置文件
vendors:
  ...
  fancybox: //cdn.jsdelivr.net/gh/fancyapps/fancybox@3/dist/jquery.fancybox.min.js
  fancybox_css: //cdn.jsdelivr.net/gh/fancyapps/fancybox@3/dist/jquery.fancybox.min.css
```
