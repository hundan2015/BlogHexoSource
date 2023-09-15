---
title: 如何在Hexo中添加Categories页面
date: 2023-09-15 12:40:12
tags:
- 网络技术
categories:
- 博客维护与开发
---

起因是这样的。最近由于想要开始写一些东西，于是尝试使用了Hexo。其中有一个叫Vivia的主题深得我爱。我尝试按照网上的教程添加Categories页面，但是什么也没有。我看别人用Next是存在这样的页面的，因此我感到疑惑。

**所以说到底如何在Hexo中添加一个Categories页面呢？**

Hexo中有合适的Categories页面需要以下的条件。

1. 具有Categories的layout。
2. layout定义了如何展示类别的ejs以及样式文件。

这个方法是通用的。因此我将提供一个不包含样式的实现方式，给大家做参考。

## 如何获取类别数据

我们先不谈如何进行排版，因为那个是theme的事情。我们先聊一聊如何获得类别数据。

通过对于[文档](https://hexo.io/zh-cn/docs/variables)的分析，我们可以通过直接应用页面的结构体，然后对每一个category进行遍历，并生成数据，就像下面的代码所示。

```js
<% site.categories.each( function(category){ %>
    // site.categories是一个Array，里面包含所有category的信息。
    // 获得了路径与名称信息，这样就可以到category的layout中。
    <a href="<%- url_for(category.path) %>">
        <div ><%= category.name %></div>
    </a>
<% }); %>
```
