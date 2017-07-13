---
layout: post
title: tensorflow-loss学习
categories: [loss]
tags: loss
---

> 本文深度学习tensorflow中的各个loss函数

<span id="top"></span>


## nce-loss

nce:	noise-contrastive estimation

> This objective is maximized when the model assigns high probabilities to the real words, and low probabilities to noise words. Technically, this is called Negative Sampling, and there is good mathematical motivation for using this loss function: The updates it proposes approximate the updates of the softmax function in the limit. But computationally it is especially appealing because computing the loss function now scales only with the number of noise words that we select (k), and not all words in the vocabulary (V). This makes it much faster to train. We will actually make use of the very similar noise-contrastive estimation (NCE) loss

定义：

```python
def nce_loss(weights, biases, inputs, labels, num_sampled, num_classes,
             num_true=1,
             sampled_values=None,
             remove_accidental_hits=False,
             partition_strategy="mod",
             name="nce_loss")
```

假设`nce_loss`之前的输入数据是`K`维的，一共有`N`个类，那么

```
weight.shape = (N, K)
bias.shape = (N)
inputs.shape = (batch_size, K)
labels.shape = (batch_size, num_true)
num_true : 实际的正样本个数
num_sampled: 采样出多少个负样本
num_classes = N
sampled_values: 采样出的负样本，如果是None，就会用不同的sampler去采样。待会儿说sampler是什么。
remove_accidental_hits: 如果采样时不小心采样到的负样本刚好是正样本，要不要干掉
partition_strategy：对weights进行embedding_lookup时并行查表时的策略。TF的embeding_lookup是在CPU里实现的，这里需要考虑多线程查表时的锁的问题。
```

nce_loss的实现逻辑如下：

```
_compute_sampled_logits: 通过这个函数计算出正样本和采样出的负样本对应的output和label

sigmoid_cross_entropy_with_logits: 通过 sigmoid cross entropy来计算output和label的loss，从而进行反向传播。这个函数把最后的问题转化为了num_sampled+num_real个两类分类问题，然后每个分类问题用了交叉熵的损伤函数，也就是logistic regression常用的损失函数。TF里还提供了一个softmax_cross_entropy_with_logits的函数，和这个有所区别。
```

再来看看TF里word2vec的实现，他用到nce_loss的代码如下：

```
  loss = tf.reduce_mean(
      tf.nn.nce_loss(nce_weights, nce_biases, embed, train_labels,
                     num_sampled, vocabulary_size))
```

可以看到，它这里并没有传`sampled_values`，那么它的负样本是怎么得到的呢？继续看`nce_loss`的实现，可以看到里面处理`sampled_values=None`的代码如下：

```
if sampled_values is None:
  sampled_values = candidate_sampling_ops.log_uniform_candidate_sampler(
      true_classes=labels,
      num_true=num_true,
      num_sampled=num_sampled,
      unique=True,
      range_max=num_classes)
```

所以，默认情况下，他会用`log_uniform_candidate_sampler`去采样。那么`log_uniform_candidate_sampler`是怎么采样的呢？他的实现在这里：

他会在`[0, range_max)`中采样出一个整数$k$

$$P(k) = (log(k + 2) - log(k + 1)) / log(range\_max + 1)$$

可以看到，$k$越大，被采样到的概率越小。那么在`TF`的`word2vec`里，类别的编号有什么含义吗？看下面的代码：

```
def build_dataset(words):
  count = [['UNK', -1]]
  count.extend(collections.Counter(words).most_common(vocabulary_size - 1))
  dictionary = dict()
  for word, _ in count:
    dictionary[word] = len(dictionary)
  data = list()
  unk_count = 0
  for word in words:
    if word in dictionary:
      index = dictionary[word]
    else:
      index = 0  # dictionary['UNK']
      unk_count += 1
    data.append(index)
  count[0][1] = unk_count
  reverse_dictionary = dict(zip(dictionary.values(), dictionary.keys()))
  return data, count, dictionary, reverse_dictionary
```

可以看到，`TF`的`word2vec`实现里，词频越大，词的类别编号也就越小。因此，在`TF`的`word2vec`里，负采样的过程其实就是优先采词频高的词作为负样本。

在提出负采样的原始论文中, 包括word2vec的原始C++实现中。是按照热门度的`0.75次方`采样的，这个和TF的实现有所区别。但大概的意思差不多，就是越热门，越有可能成为负样本。
                     
## 附

（1）http://www.jianshu.com/p/fab82fa53e16

[回到目录](#top)

## 引

(1) [The Unreasonable Effectiveness of Recurrent Neural Networks](http://karpathy.github.io/2015/05/21/rnn-effectiveness/)
(2) [word2vec](https://www.tensorflow.org/tutorials/word2vec)