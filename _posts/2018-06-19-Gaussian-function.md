---
layout: post
title: 高斯模糊
categories: [图像检索]
tags: 高斯 gaussian
---

<!--<span id="top"></span>

macdown下使用这条命令去生成toc

[toc] -->


* TOC
{:toc}

## 正态分布

正态分布（Normal distribution），也称“常态分布”，又名高斯分布（Gaussian distribution），最早由A.棣莫弗在求二项分布的渐近公式中得到。C.F.高斯在研究测量误差时从另一个角度导出了它。P.S.拉普拉斯和高斯研究了它的性质。是一个在数学、物理及工程等领域都非常重要的概率分布，在统计学的许多方面有着重大的影响力。
正态曲线呈钟型，两头低，中间高，左右对称因其曲线呈钟形，因此人们又经常称之为钟形曲线。

$$
\begin{equation}
   f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{\frac{-(x-\mu)^2}{2\sigma^2}}
\end{equation}
$$

若随机变量X服从一个数学期望为μ、方差为σ^2的正态分布，记为N(μ，σ^2)。其概率密度函数为正态分布的期望值μ决定了其位置，其标准差σ决定了分布的幅度。当μ = 0,σ = 1时的正态分布是标准正态分布。

$$
\begin{equation}
   f(x)=\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}
\end{equation}
$$

## 高斯函数

上面的正态分布是一维的，图像都是二维的，所以我们需要二维的正态分布。
因为计算平均值的时候，中心点就是原点，所以μ等于0。

$$
\begin{equation}
   G(x,y)=\frac{1}{\sqrt{2\pi}\sigma}e^{\frac{-(x^2+y^2)}{2\sigma^2}}
\end{equation}
$$

## 附

[回到目录](#markdown-toc)

## 引用
