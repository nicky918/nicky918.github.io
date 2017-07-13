---
layout: post
title: 数据降维方法
categories: [rnn]
tags: rnn
---

> 如果我们的目标是计算每一对文档对之间的相似度，那么我们就没有更好的办法了，或者可以用并行的方法减少运行的时间规模；但是如果我们的目的仅仅是找到的最相似的文档对或者相似度在某种程度之上的文档对，这时候我们就可以采用LSH技术（locality-sensitive hashing or near-neighbor search）。

<span id="top"></span>

## LSH(Locality-Sensitive Hashing)

在很多应用领域中，我们面对和需要处理的数据往往是海量并且具有很高的维度，怎样快速地从海量的高维数据集合中找到与某个数据最相似（距离最近）的一个数据或多个数据成为了一个难点和问题。如果是低维的小数据集，我们通过线性查找（Linear Search）就可以容易解决，但如果是对一个海量的高维数据集采用线性查找匹配的话，会非常耗时，因此，为了解决该问题，我们需要采用一些类似索引的技术来加快查找过程，通常这类技术称为最近邻查找（Nearest  Neighbor,AN），例如K-d tree；或近似最近邻查找（Approximate Nearest  Neighbor, ANN），例如K-d tree with BBF, Randomized Kd-trees, Hierarchical K-means Tree。而LSH(局部哈希敏感)是ANN中的一类方法。

原理：
这些`hash function`需要满足以下两个条件：

* 如果$d(x,y) ≤ d1$， 则$h(x) = h(y)$的概率至少为$p1$；
* 如果$d(x,y) ≥ d2$， 则$h(x) = h(y)$的概率至多为$p2$；

其中$d(x,y)$表示x和y之间的距离，$d1 < d2$， $h(x)$和$h(y)$分别表示对x和y进行hash变换。
满足以上两个条件的hash functions称为$(d1,d2,p1,p2)-sensitive$。而通过一个或多个$(d1,d2,p1,p2)-sensitive$的hash function对原始数据集合进行hashing生成一个或多个hash table的过程称为`Locality-sensitive Hashing`。

文献：
[Practical and Optimal LSH for Angular Distance](../documents/5893-practical-and-optimal-lsh-for-angular-distance.pdf)

## 附

[回到目录](#top)

## 引

（1）[PCA的数学原理](http://blog.csdn.net/xiaojidan2011/article/details/11595869)
（2）[原文](http://blog.codinglabs.org/articles/pca-tutorial.html)