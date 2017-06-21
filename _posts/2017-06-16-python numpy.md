---
layout: post
title: python：numpy
categories: [python]
tags: python
---

> NumPy（Numerical Python的简称）是高性能科学 和数据分析的基础包

## 综述

其重要功能如下： 
1. ndarray，一个具有矢量运算和复杂广播能力的快速且节省空间的多维数组。 
2. 用于对数组数据进行快速运算的标准数学函数（无需编写循环）。 
3. 线性代数、随机数生成以及傅里叶变换功能。
4. `import numpy as np`
5. [官方文档](https://docs.scipy.org/doc/numpy/reference/)
6. [中文文档](http://python.usyiyi.cn/translate/NumPy_v111/index.html)
7. [详细目录](https://docs.scipy.org/doc/numpy/contents.html)

## 数据创建函数

`NumPy`的主要对象是同质多维数组。它是一张表，所有元素（通常是数字）的类型都相同，并通过正整数元组索引。在NumPy中，维度称为轴。轴的数目为秩`rank`。

例如，3D空间中的点的坐标`[1, 2, 1]`是rank为1的数组，因为它具有一个轴。该轴的长度为3。在下图所示的示例中，数组的rank为2（它是2维的）。第一维度（轴）的长度为2，第二维度的长度为3。

```
[[ 1., 0., 0.],
 [ 0., 1., 2.]]
```

NumPy的数组的类称为`ndarray`。别名为`array`。请注意，`numpy.array`与标准`Python`库的类`array.array`不同，后者仅处理一维数组并提供较少的功能。`ndarray`对象的更重要的属性是：

`ndarray.ndim`

数组的轴（维度）的个数。在`Python`世界中，维度的数量被称为`rank`。

`ndarray.shape`

数组的维度。这是一个整数的元组，表示每个维度中数组的大小。对于具有`n`行和`m`列的矩阵，`shape`将是`(n,m)`。因此，shape元组的长度就是`rank`或维度的个数`ndim`。

`ndarray.size`

数组元素的总数。这等于`shape`的元素的乘积。

`ndarray.dtype`

描述数组中元素类型的对象。可以使用标准Python类型创建或指定`dtype`。另外`NumPy`提供了自己的类型。例如`numpy.int32`、`numpy.int16`和`numpy.float64`。

`ndarray.itemsize`

数组中每个元素的字节大小。例如，元素为`float64`类型的数组的itemsize为`8（=64/8）`，而`complex32`类型的数组的`comitemsize`为`4（=32/8）`。它等于`ndarray.dtype.itemsize`。

`ndarray.data`

该缓冲区包含数组的实际元素。通常，我们不需要使用此属性，因为我们将使用索引访问数组中的元素。

```python
import numpy as np

data = [1.1,2.2,3.3]
data = np.array(data)

print data                           #[ 1.1  2.2  3.3]
print data.dtype                     #float64
print data.shape                     #(3L,)

data = data*10                       #不同于python里面列表的运算
print data                           #[ 11.  22.  33.]

data = data.astype(np.int16)
print data.dtype                     #int16
```

```
[ 1.1  2.2  3.3]
float64
(3,)
[ 11.  22.  33.]
int16
```

## 元素级数组函数

```python
arr = np.arange(10)
print arr           #[0 1 2 3 4 5 6 7 8 9]
print np.sqrt(arr)
#[ 0. 1. 1.41421356 1.73205081 2. 2.23606798  2.44948974 2.64575131 2.82842712 3.]

x = np.random.randn(8)
y = np.random.randn(8)
print x
print y
print np.maximum(x,y)

#输出
# [  6.44384037e-01   4.77467240e-01  -1.63710500e+00  -2.23397938e-01
#    2.42823340e-04   2.90417057e-01   1.59116209e+00  -9.84905419e-01]
# [-0.83284853  1.37952239  0.21118756 -0.17092715  0.01228589  1.54071412
#   1.44245279  0.63661402]
# [ 0.64438404  1.37952239  0.21118756 -0.17092715  0.01228589  1.54071412
#   1.59116209  0.63661402]
```

## 基本数组统计方法

## 数据的集合运算

```
numpy.unique    查找数组的唯一元素。
```

## 常用的numpy.linalg函数（Linear algebra）

```
dot（a，b [，out]）	两个数组的点积。
linalg.norm（x [，ord，axis，keepdims]）	矩阵或向量范数。
linalg.solve（a，b）	求解线性矩阵方程或线性标量方程组。
linalg.tensorsolve（a，b [，axes]）	为x解出张量方程 ax = b 
linalg.lstsq（a，b [，rcond]）	将最小二乘解返回到线性矩阵方程。
linalg.inv（a）	计算矩阵的（乘法）逆。
linalg.pinv（a [，rcond]）	计算矩阵的（Moore-Penrose）伪逆。
linalg.tensorinv（a [，ind]）	计算N维数组的“逆”。
```

## 部分numpy.random函数

## 数组连接函数

## take、where、copy

`take`

从轴沿一个数组中取元素。

参数：
a：源数组。indices：要提取的值的索引。

```
arr = np.arange(10)*100
inds = [7,1,2,6]
print arr.take(inds)      #[700 100 200 600]
```

`where`

numpy.where函数是三元表达式x if condition else y的矢量化版本。

```
arr = np.random.randn(4,4)
print arr

[[ 1.19717763 -0.741477   -2.15110668 -0.46468596]
 [-1.13466991  1.10899501 -1.61028763 -0.6177533 ]
 [ 1.95222784 -0.00466218 -1.23314358  0.75340766]
 [ 0.24350204 -0.23099947  0.11610512  0.21914577]]

print np.where(arr < 0, 0, arr)

[[ 1.19717763  0.          0.          0.        ]
 [ 0.          1.10899501  0.          0.        ]
 [ 1.95222784  0.          0.          0.75340766]
 [ 0.24350204  0.          0.11610512  0.21914577]]
```

`copy`

```
data_copy = data.copy（） # 可以得到一份副本，并不影响原来的数据
```

## 引用

1. [http://blog.csdn.net/a819825294/article/details/53200601](http://blog.csdn.net/a819825294/article/details/53200601)
