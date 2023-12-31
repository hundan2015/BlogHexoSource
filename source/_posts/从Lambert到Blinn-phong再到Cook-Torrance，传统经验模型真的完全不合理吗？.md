---
title: 从Cook-Torrance看Lambert到Blinn-phong，传统经验模型真的完全不合理吗？
date: 2023-10-25 18:18:50
tags:
- CG
categories:
- 计算机图形学
---
## 动机

最近出于各种原因，我在看一些经典的光照模型：Lambert、Phong还有Blinn-Phong。当然，经典的PBR渲染公式也不能忘记。什么微表面理论，菲涅尔项之类的。说到微表面模型，就不能忘记GGX。在GGX中有一个关键的计算，就是需要取一下视线向量和光线向量的加和，然后再取这个加和得到的向量与平面法线向量的夹角。很巧合的是，Blinn Phong模型作为一个经验模型也采用了一个类似的运算。那么这里有什么关系呢？我自己尝试使用微表面理论进行解释，一点问题都没有。我就在想，用一个所谓未来理论的假说解释一个过去古老的经验模型，是否有些不妥。我便拿来了原来提出Blinn Phong模型的论文，才发现倒错的是我。Cook-Torrance模型早于Blinn Phong模型，并且Blinn Phong模型的合理性就是由Cook-Torrance模型进行解释。这种反差让我感受到了一种时代的错位感，就像是你发现并不是先有谭浩强后有C++一样离谱。但是有没有可能只是我们忘记了历史，忘记了计算机图形学的发展脉络呢？我认为，**伟人们早在他们的时代便提出了超前的构思，而又陷于时代只能假设一些经验前提来简化计算，进而有了当时所谓的经验模型。** 因此我便想通过这篇文章，从历史的角度，揣度或重现过去学者对于光照模型的构思，并从此窥见光照模型发展的脉络。

@[toc]

## 研究的入手点与方法

本篇文章主要集中于Lambert、Phong以及Blinn-Phong三个经典经验模型，涉及到微表面理论，以及部分辐射度量学。Blinn-Phong的原论文非常精彩，甚至十分超前。在本篇文章内，我们从1986年的渲染方程理论入手，结合辐射度量学，对比当时他们提出的理论与后来渲染方程之间的关系，探究这三个模型做出的经验性假设。

## 相关工作

### 微表面模型

### 简单能量守恒

对于受到辐射的物体，辐射本身的能量被分为了两部分。一部分被物体吸收，然后通过光线在物体中的碰撞再以某种形式散发出来，这种形式可能是光能或者热能。还有部分的能量被物体表面反射，进入到我们的人眼中。对于这两个不同的能量部分，我们将反射到我们眼中的光称为Specular，也就是镜面反射光。经过物体内部折射然后从物体内部发出的光称为Diffuse，也就是漫反射光。

### 渲染方程

渲染方程如下所示：

$$
L_o(x,\omega_o,\lambda)=L_e(x,\omega_o,\lambda)+L_r(x,\omega_o,\lambda)\\
L_r(x,\omega_o,\lambda) = \int_\Omega f_r(x,\omega_i,\omega_o,\lambda)L_i(x,\omega_i,\lambda)(\omega_i ·n) \mathrm{d}\omega_i
$$

在其中，几个符号这么解释：

* $x$为被光照射或者被观察到的点；
* $\lambda$为光的波长；
* $\omega_o$为观察者眼睛到被观察点的向量；$\omega_i$为入射光打在被观察点的向量。

方程整体上这个又分成了两个部分：

* $L_e$和$L_r$。$L_e$对应着能量守恒理论中的漫反射以及物体自发光的部分；
* $L_r$则对应着能量守恒中的镜面反射部分。

在$L_r$的部分，又有三个大块：

* $f_r$代表着一个物体的BRDF，也就是双向反射分布函数。他代表着通过镜面反射从$\omega_o$方向射出的光线相对于从$\omega_i$方向射向点$x$的，波长为$\lambda$的光线所含能量的倍数。
* $L_i$则代表着从$\omega_i$射向$x$点的波长为$\lambda$的光线所含能量大小。
* 最后$(\omega_i ·n)$为Lambert系数项。因为光线与观察面优势并不是垂直的，观察面所接受的irradiance需要通过光线与法线的夹角的余弦值来进行修正。

### Cook-Torrance模型

Torrance模型通过一个假设理论来描述渲染方程的$f_r$项。其表达大致如下所示：

$$
f_{Cook-Torrance}=\frac{DFG}{4(\omega_o ·n)(\omega_i ·n)}
$$

对于模型来说，主要分成3个部分：

* D：每单位面积,每单位立体角所有法向为$h$的微平面的面积
* F：描述了物体表面在不同入射光角度下反射光线所占的比率
* G：描述了微平面自遮挡的属性。

但下面的参数究竟是怎么来的？这里涉及到一些辐射度量学和数学知识，建议参考[这篇文章](https://zhuanlan.zhihu.com/p/152226698)进行推导。从结果来看，下面的参数是我们针对D项做的一些修正。

## 从现代角度对传统光照模型的分析

### Lambert

Lambert模型算是十分古老的模型，也是最简单的光照模型。Lambert模型发现了一个基本的要素：法线面向光源的面越亮，法线越垂直于光源的面越暗。在Lambert模型中，这个亮度和$cos\theta$——也就是照射到表面的光线方向与表面法线方向的余弦值正相关。

一个粗糙的模型可以大道至简。对于亮与暗的变化我们完全可以用一个线性的关系来描述。但为什么在Lambert模型里我们要用$cos\theta$？我们从辐射度量学的角度来看这件事。对于一个单位面积上的单位立体角$\mathbf{d}A\mathbf{d}\omega_h$，根据radiance的定义$\frac{\mathbf{d}^2\phi}{cos\theta\mathbf{d}\omega\mathbf{d}A}$得知，当前表面的Irradiance为$L_i  cos\theta$，其中$\theta$为法线与光照方向的夹角。通过辐射度量学，我们解释了为什么光照不是线性变化的，而是使用$cos\theta$描述。

从渲染方程的角度，我们再看看Lambert模型对于漫反射的事物做了什么样的假设。我们将Lambert模型的表达带回到渲染方程中，可以很轻易地计算出Lambert模型对应的BRDF为$\frac{c}{\pi}$ [^1]，其中c为物体自带的固有色。也就是说，Lambert模型假设从表面上向任意方向反射的光的强度是相同的。

综上所述，Lambert模型基于以下的现象、理论与假设：

[^1]: 这里我们为了能量守恒，除以一个$\pi$。Lambert模型最简单的实现版本是直接使用余弦来计算亮度，但这样会使得物体放出的能量比它接收要多。不过我们同样可以通过辐射度量学，使用一个线性系数$\frac{1}{\pi}$让其变得能量守恒，修正亮度爆炸的问题。

> **现象**：
>
> * 法线面向光源的面越亮，法线越垂直于光源的面越暗。
>
> **理论：**
>
> * 辐射度量学
>   * 一个面上接受的能量与光线与表面法线夹角的余弦成正比。
>
> **假设：**
>
> * 从表面上向任意方向反射的光的强度是均匀的。

虽然这个世界上不存在均匀向四周反射光线的物体，但这种假设也依然符合我们对于理想漫反射物体的认知。这十分的有趣。使用$cos\theta$构建明暗关系说明当时的学者已经意识到辐射度量学对于CG的意义，并基于这个理论构建模型。可以说，Lambert模型为光照领域引入了辐射度量学的分析方法，为我们日后如何解决如何Shading的问题提供了基础。

### Phong与Blinn-Phong

Phong与Blinn-Phong模型观察到了一个现象：比较光滑的漫反射物体在表面法线与光线方向几乎相同的位置具有高光。

Phong与Blinn-Phong是相较于Lambert模型更良好的模型，但在很多资料上并没有证明为什么他们为什么更好，尤其是Blinn-Phong相较于Phong。也许是因为这个原因，通常这三个模型的发展过程被认为是经验上的改进。但其实从Blinn-Phong的原论文来看，Blinn-Phong模型已经运用上了微表面理论与初级的Torrance模型。因此我们需要从现象、理论以及假设三个角度重现Blinn-Phong模型的推导，然后从理论的角度分析为什么它更好。

Lambert模型虽然对于漫反射物体有着直观的模拟，但是它不能很好地拟合塑料上的高光。Phong模型的提出就是为了解决这个问题。我们来看看他们怎么构建Phong模型的。

> **现象**：
>
> * 法线面向光源的面越亮，法线越垂直于光源的面越暗。
> * 法线靠近光源的面具有光泽
> * 只有一小部分面有高光，远离光源的面几乎没有高光
>
> **理论：**
>
> * 辐射度量学
>   * 一个面上接受的能量与光线与表面法线夹角的余弦成正比。
>
> **假设：**
>
> * 从表面上向任意方向反射的光的强度是均匀的。
> * 法线与光源的夹角越小，面越亮。
>

基于这个假设，Phong模型首先计算了光照的直接反射方向。然后求这个方向与视线向量构成的角的余弦值。为了模拟远离光线的面没有高光的效果，Phong模型将余弦值进行幂运算，进而获得了一个凹函数，且在前半部分的斜率绝对值十分大。

虽然只是需要多存储一个反射之后的夹角，这种计算方式还是挺麻烦的。通过引入微表面理论，Blinn提出了一个更简便，且更加正确的算法。

> **现象**：
>
> * 法线面向光源的面越亮，法线越垂直于光源的面越暗。
> * 法线靠近光源的面具有光泽
> * 只有一小部分面有高光，远离光源的面几乎没有高光
>
> **理论：**
>
> * 辐射度量学
>   * 一个面上接受的能量与光线与表面法线夹角的余弦成正比。
> * 微表面模型
>   * 对于一个宏表面，设光源放行与视线向量加和的方向为h，其中只有法线方向与h相同的微表面才反射光。
>
> **假设：**
>
> * 从表面上向任意方向反射的光的强度是均匀的。
> * 法线与h的夹角越小，面越亮。
>

通过对于微表面理论的使用，可以看出，Blinn在改进Phong模型的时候是完全符合理论的。虽然最后也是需要使用一个幂函数来构建凹函数，但是这种情况也更加合理。

不可思议的是，在当时Blinn就在相关的论文中提供了Torrance模型视角的解释。而且提供了与现代游戏引擎中普遍使用的D函数。这个D函数更加容易计算，且更加准确。Blinn-Phong模型的提出，我更多认为是在Phong这个主流的框架下的一种改进。这个改进对于大众更好接受且效率更好，进而从这篇论文中被抽离出来。对于这篇文章更精彩的部分应该是在后面关于Torrance模型与为微表面模型的理论证明。如果感兴趣请务必看看这篇论文。

## 写在后面

原本这篇文章就是火花一现，我以为我离CG历史更近了一步。本来我想通过研究历史来看清未来的发展方向，但通过资料的查找发现自己只能止步于此。我们如今活在1980年代科技爆炸的余波中，现有的新理论并没有突破当时的框架。这令人感到绝望，因为我知道自己不是创造爆炸的人，只能期待伟人的出现并在这之前以框架上的残肉勉强度日。但我也觉得不应如此消沉，毕竟我的目的也便是希望能找到能创造更好方法的方法。这次尝试虽然失败了，但也应该保持好奇与探索，保持归纳总结与抽象。希望未来的我能更加强力。

## 注释与引用
