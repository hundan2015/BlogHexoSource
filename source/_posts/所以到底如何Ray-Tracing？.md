---
title: 所以到底如何Ray Tracing？
date: 2023-09-15 17:26:05
tags:
- CG
- C++
categories:
- 计算机图形学
---
## 写在前面

Ray tracing算是渲染领域中很经典的话题了。这个算法突出的就是一个纯纯的暴力，但这也是目前最真实的算法。而且，即便暴力，为了实现更快的速度，无数的前辈们也是在用各种统计上的方法还有各种数学组合来使得运算变得更加快速。这从最初的Whitted Style到现在的Path Tracing，全都是各位前辈取得的成果。

问题就来了，为什么学做Ray tracing？Ray tracing作为一个古老的算法，是基本符合人的认知的。也就是说，Ray tracing本身的思想并不复杂。在我自己学习的时候，我从RT的角度对很多光栅化里使用的方法有了更好的理解。**简单来说，Ray tracing适合没有接触过图形学相关概念的新手。**

虽然如此，Ray tracing渲染器依然是一个复杂的工程。当需要大量的概念聚合起来的时候，便要考虑设计，考虑性能云云。而且现在没有一个很基础的API（比如OpenGL）来进行Ray tracing。即便是有，也是基本抽象过了头，对于理解RT本身并无好处。

为了更好理解RT，这篇文章将会以RT中涉及到的概念进行讲解。其中会简单涉及到一些工程上的设计，我会尝试说明这些设计的合理性。最后通过讲解，来明白Ray tracing的基础过程。

顺便一提，如果有兴趣进行实践的话，请务必观看[RayTracing weekend](https://raytracing.github.io/)系列。

<!-- toc -->

## 辐射度量学基础

在这一节中，我们将围绕着Radiance，Irradiance，Radio intansity这三个概念，介绍他们之间的关系。

### Radio Intensity

### Radiance

### Irradiance

## 渲染方程与蒙特卡洛方法

在这一节中，我们将介绍渲染方程的基本组成，并从计算的角度简单介绍蒙特卡罗方法。

## 从光线追踪到路径追踪

在这一节中，我们将介绍基本的光线追踪方法，通过对计算量的了解，解释为什么要使用路径追踪。

## 工程解：Ray、Hit record以及Scatter

在这一节中，我们将引入Ray tracing weekend中的Ray、Hit record以及Scatter这三个设计。并且将他们与光栅化中的部分概念进行比较，分析他们的思路。

## Lambert光照模型

在这一节中，我们将介绍Lambert光照模型，并分析其与渲染方程之间的关系。
