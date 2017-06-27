---
layout: post
title: 图像检索：布料图像索引及检索
categories: [图像检索]
tags: 图像检索
---

> 遇到问题，先用已知的知识和成果，给出最好的方案。再去进一步探索现有方案的优点与不足，了解算法的本质。进而朝着你期望的方案不断去探索新的好的方案而努力

<span id="top"></span>

## 目录

* [问题](#1) 
* [框架](#2) 
* [CEDD](#3)
* [探索其他的方法](#4)
* [SIFT](#5)
* [最终流程](#8)

<h2 id="1">问题</h2>

|项目|详述|
|:--:|:--:|
|图库大小|30W->100W+，目前70W|
|目标|通过拍照的图片进行检索，根据相似度列出搜索结果|

<h2 id="2">框架</h2>

[LIRE: Lucene Image Retrieval](#lire)
> LIRE is a Java library that provides a simple way to retrieve images and photos based on color and texture characteristics. LIRE creates a Lucene index of image features for content based image retrieval (`CBIR`) using local and global state-of-the-art methods. Easy to use methods for searching the index and result browsing are provided. Best of all: it's all open source.

<h2 id="3">CEDD全局算子</h2>

[图像检索：CEDD（Color and Edge Directivity Descriptor）算法](http://blog.csdn.net/leixiaohua1020/article/details/16883379)

`CEDD`的英文全称是Color and Edge Directivity Descriptor，即颜色和边缘方向特征描述符。它结合了图像的颜色和纹理信息，生成一个(`24*6`)位的直方图。这个特征提取方法可以分为两个子模块系统，提取颜色信息的是颜色模块，提取纹理信息的是纹理模块。CEDD直方图信息由六个区域组成，也就是纹理模块，六个区域就是提取出的6维向量直方图，然后在这些纹理信息的每一维中再加入颜色模块提取出的24维颜色信息，这样就可以将颜色和纹理有效结合起来，最终得出**6*24=144**维的直方图信息。

直方图中各维信息的含义分别是：（0）无边缘信息，（1）无方向的边缘信息，（2）水平方向的边缘信息，（3）垂直方向的边缘信息，（4） 45度方向的边缘信息，（5） 135度方向的边缘信息。

10-bins直方图信息值的含义如下：（0）黒色（Black），（1）灰色（Gray），（2）白色（White），（3）红色（Red）, （4）橙色（Orange），（5）黄色（Yellow），（6）绿色（Green），（7）青色（Cyan），（8）蓝色（Blue），（9）品红色（Magenta）。

24-bins模糊过滤器就是将10-bins模糊过滤器输出的每种色区再分为3个H值区域，输入一个10维向量和S、V通道值，输出的是一个24维向量。它输出的每一维所代表的信息分别是：（0）黑色（Black），（1）灰色（Grey），（2）白色（White），（3）暗红色（Dark Red），（4）红色（Red），（5）浅红（Light Red）,（6）暗橙色（DarkOrange），（7）橙色（Orange），（8）浅橙色（Light Orange），（9）暗黄色（Dark Yellow），（10）黄色（Yellow）, （11）浅黄色（LightYellow），（12）深绿色（Dark Green），（13）绿色（Green），（14）浅绿色（Light Green），（15）暗青色（Dark Cyan），（16）青色（Cyan），（17）浅青色（Light Cyan），（18）深蓝色（Dark Blue）,（19）蓝色（Blue），（20）淡蓝色（LightBlue），（21）暗品红色（DarkMagenta），（22）品红色（Magenta），（23）浅品红色（Light Magenta）。

![](../images/posts/2017/cedd.png)

> 搜索效果：

![](../images/posts/2017/cedd-1.png)

![](../images/posts/2017/cedd-2.png)

![](../images/posts/2017/cedd-3.png)

![](../images/posts/2017/cedd-4.png)

![](../images/posts/2017/cedd-5.png)

> 结论：
> 色彩效果很不错，但是结构纹理不能保证，聚类效果差

<h2 id="4">探索其他的方法</h2>

[已有其他的一些较好算子](http://192.168.21.10/docs/show/432)

<h2 id="5">SIFT局部算子</h2>

SIFT（Scale-invariant feature transform）是一种检测局部特征的算法，该算法通过求一幅图中的特征点（interest points,or corner points）及其有关scale 和 orientation 的描述子得到特征并进行图像特征点匹配，获得了良好效果，详细解析如下：

> 算法描述 

SIFT特征不只具有尺度不变性，即使改变旋转角度，图像亮度或拍摄视角，仍然能够得到好的检测效果。整个算法分为以下几个部分： 

1. 构造尺度空间：DoG尺度空间 
1. 检测DoG尺度空间极值点 
1. 去除不好的特征点 
1. 给特征点赋值一个128维的方向参数。每个关键点都包含三个信息：位置、尺度和方向。 
1. 关键点描述子的生成： 
1. 首先将坐标轴旋转为关键点的方向，以确保旋转不变性。以关键点为中心取8×8的窗口。 
1. 最后进行特征匹配。当两幅图像的SIFT特征向量（128维）生成后，采用关键点特征向量的欧式聚类来作为相似性判定度量。 

<h2 id="6">SIFT算法调整</h2>

### 图片size

[图片大小对SIFT特征提取的影响](http://192.168.21.10/docs/show/509)

### sift算法应用

![bow介绍](../blog/CBIR-BoW-for-image-retrieval-and-practice.html)

再者来看看opencv的实现

```cpp
SIFT::SIFT(int nfeatures=0, int nOctaveLayers=3, double contrastThreshold=0.04, double edgeThreshold=  
10, double sigma=1.6)
```

在sift算法中有几个参数：

    nfeatures：特征点数目（算法对检测出的特征点排名，返回最好的nfeatures个特征点）。
    nOctaveLayers：金字塔中每组的层数（算法中会自己计算这个值，后面会介绍）。
    contrastThreshold：过滤掉较差的特征点的对阈值。contrastThreshold越大，返回的特征点越少。
    edgeThreshold：过滤掉边缘效应的阈值。edgeThreshold越大，特征点越多（被多滤掉的越少）。
    sigma：金字塔第0层图像高斯滤波系数，也就是σ。
	

edgeThreshold调整

![](../images/posts/2017/sift-1.png)
![](../images/posts/2017/sift-2.png)
![](../images/posts/2017/sift-3.png)
![](../images/posts/2017/sift-4.png)
![](../images/posts/2017/sift-5.png)
![](../images/posts/2017/sift-6.png)
![](../images/posts/2017/sift-7.png)

<h2 id="8">最终流程</h2>

![](../images/posts/2017/flow-1.png)

# 引用

1. 【<span id="lire">[**LIRe**](http://www.lire-project.net/),[**github**](https://github.com/dermotte/lire)</span>】
2. 【[图像检索：CEDD（Color and Edge Directivity Descriptor）算法](http://blog.csdn.net/leixiaohua1020/article/details/16883379)】
3. 【[Self-Normalizing Neural Networks](https://arxiv.org/abs/1706.02515)】

## 附

[回到目录](#top)