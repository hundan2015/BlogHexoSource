---
title: 如何在Hexo中添加Categories页面
date: 2023-09-15 12:40:12
tags:
- 网络技术
---

起因是这样的。最近由于想要开始写一些东西，于是尝试使用了Hexo。其中有一个叫Vivia的主题深得我爱。我尝试按照网上的教程添加Categories页面，但是什么也没有。我看别人用Next是存在这样的页面的，因此我感到疑惑。

所以说到底如何在Hexo中添加一个Categories页面呢？

Hexo中有合适的Categories页面需要以下的条件。

1. 具有Categories的layout。
2. layout定义了如何展示类别的ejs以及样式文件。

Vivia是通过landscape改来的主题，所以说我以Vivia为例子说明一下这个东西的改进方法。
