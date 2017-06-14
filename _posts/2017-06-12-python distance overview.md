---
layout: post
title: python：距离实现综述
categories: [机器学习]
tags: python
---

> 积累+学习

## 综述

所列的距离公式列表和代码如下：

* 闵可夫斯基距离(Minkowski Distance)
* **欧氏距离(Euclidean Distance)**
* **曼哈顿距离(Manhattan Distance)**
* 切比雪夫距离(Chebyshev Distance)
* 夹角余弦(Cosine)
* 汉明距离(Hamming distance)
* 杰卡德相似系数(Jaccard similarity coefficient)

读者可根据自己需求有选择的学习。因使用矢量编程的方法，距离计算得到了较大的简化。

其中欧氏距离与曼哈顿距离是比较常用的

## 欧氏距离

![](../images/posts/2017/Euclidean.png)

python 源码 - 三种实现

```python
import numpy as np

vect1 = np.mat([1,2,3]);
vect2 = np.mat([2,3,4]);

dis = np.sqrt((vect1 - vect2) * (vect1 - vect2).T)

#sqrt(3)
print dis

dis = np.sqrt( np.sum( np.square(vect1 - vect2) ) )

print dis

dis = np.linalg.norm(vect1 - vect2)

print dis
```

## 曼哈顿距离(Manhattan Distance)

![](../images/posts/2017/manhattan.png)

```python
import numpy as np

vect1 = np.mat([1,2,3])
vect2 = np.mat([2,3,4])

dis = np.sum( np.abs(vect1-vect2) )

#3
print dis
```

## 引用

[python 各类距离公式实现](http://blog.csdn.net/guojingjuan/article/details/50396254)