---
layout: post
title: "MIT CS246: Mining Massive Datasets - Chapter 3: Finding Similar Items"
author: Dervean
description: "MIT CS246"
categories: [data_mining]
tags: [data_mining]
redirect_from:
  - /2018/06/16/
---

[MIT CS246: Mining Massive Datasets](http://web.stanford.edu/class/cs246/)

---

* Kramdown table of contents
{:toc .toc}

# Overview

问题概述：如何检查哪些数据是相似的？例如如何检查论文抄袭。

在大规模数据背景下，本章给出了一个技术路线（如下图所示）来解决上述问题。

![big-picture](/images/mining-massive-datasets/chapter3-big-picture.png "Big picture")

简单来说分为三步，
 
- Shingling: Convert documents to sets. 

- Min-Hashing: Convert large sets to short signatures, while preserving similarity

- Locality Sensitive Hashing: Focus on pairs of signatures likely to be from similar documents

下面以检测论文抄袭为例来说明这个方法，我们只考虑论文中的文字部分，不考虑如图片等非文字部分。

# Shingling

在检测论文抄袭问题里，**shingling** 就是把每篇论文给拆分成一个个字母（或者单词）组成的部分（term），每个 term 是 k 个连续的字母（或者单词）。

我们希望把每篇文章看成一个 term set，然后根据两个 set 的重叠率来判断两篇文章是否相似，这里使用的是 **Jaccard similarity**:
	
$$
Sim(C_1, C_2) = \frac{|C_1 \cap C_2|}{|C_1 \cup C_2|}
$$

我们只考虑集合，也就是每篇文章的 term 种类，而不考虑每个 term 的个数。显然如果两篇文章所用的 term 完全一样（或者大部分一样），则可以判断二者相似。

那为什么使用 term 而不使用单词呢？因为单词的种类数目有限，例如英文的常用词汇个数只有数千个，两两文章使用单词重叠的概率将会比较大而且区分度低。使用 Shingling 可以提高 terms 的种类数目，一般来说 k 越大 terms 的种类数目越大。

如何确定 k？
- 显然如果 k 太小，机器可能会判断很多文章都是类似的，这种情况下区分度也很低，如果 k 太大，结果则相反（绝大部分文章都不类似，也同样没有区分度）。

- "k should be picked large enough that the probability of any given shingle appearing in any given document is low. "

- 一般对于检测论文 k = 9 就够了。

在实际计算过程中，我们不可能直接使用 term set 来比较，毕竟字符串占用空间太大，如果有 100 万篇文章，光存储问题都没法解决。为了减少空间占用，我们可以将 term set 散列到整数空间上(例如 4 个字节，0 $\sim$ $2^{32} - 1$)。假设一篇文章总共 5000 个单词，含有 term 种类共 4800 个，如果 k=9 ，即便每个 term 只占 9 个字节，也需要 $4800 \times 9$ 个字节，但使用 hash 只需 $4800 \times 4$ 个字节就可以表示该文章。

我们也可以把每个 term 看作“特征”，每篇文章就可以表示为一个特征向量，然后比较两两向量是否相似：

| shingle   | D_1	| D_2	| D_3 | ... |
| ----- 	|-|-|-|-|
| s_1		|1|1|1|...|		
| s_2		|1|1|0|...|	
| s_3		|0|1|0|...|	
| s_4		|0|0|0|...|	
| s_5		|1|1|0|...|	
| s_6		|1|0|1|...|
|...		|...|...|...|...|

现在我们来总结一下存在的问题。

第一是数据量太大了，100 万篇论文，需要比较次数:

$$
C_{1000000}^2 \approx 5 * 10^{11}
$$

如果是 1000 万篇论文，光执行比较操作就需要一年！显然我们需要别的方法来加快比较的速度。

第二是上面得到的特征矩阵是一个稀疏矩阵，虽然可以用来可视化数据，但实际操作的时候这种方式空间利用率太低。

# Min-Hashing

上面提到可以使用 hash 来降低每篇文章的空间占用量，但是当文章数目过多，空间占用量还是很大。如果能减少每篇文章的空间占用，一来可以节省存储空间，二来也可以加快比较的速度（内存可以容纳更多的文章）。

现在来做个类比，在实际案件侦破中会根据**犯罪现场留下的指纹**和**对犯人的侧写**来搜寻犯人，“指纹”对应一个人，“侧写”也对应一个人，但是“指纹”相比较“侧写”更简洁。

如果把特征向量来比作文章的“侧写”，这种“侧写”过于具体和冗余，其中很大一部分都不是必要的内容（向量中绝大部分为 0），那现在能不能根据“侧写”得到文章的“指纹”，使用一个更简洁的东西来代表文章。这个过程就是 Min-Hashing。

简单来说，Min-Hashing 是对原有的特征矩阵做了置换之后，从得到的新矩阵总结出新的信息来代替原矩阵，而且保证在此过程没有破坏矩阵中原有的列与列之间的相似关系。具体过程如下:

1. 随机选择一个置换矩阵 $P$；

2. 对原特征矩阵 $M$ 作置换操作 $P$ 得到 $M'$，选择 $M'$ 的每列的第一个元素为“1”对应的行号作为该列的 Min-Hashing 值；

3. 重复步骤 1 - 2 多次，得到 Signature matrix，signature 即新矩阵的一个列向量；

	![chapter3-minhashing-example](/images/mining-massive-datasets/chapter3-minhashing-example.png "Min-hashing example")


我们来比较上例对原有 columns 计算相似度以及对 Signature matrix 计算相似度的结果:

| 		| 1-3 	| 2-4 	| 1-2 	| 3-4 	|
|-		|-		|-		|-		|-		|
|Col/Col|0.75	|0.75	|0		|0		|	
|Sig/Sig|0.67	|1.00	|0		|0		|	

值得一提的是，在实际计算的过程中我们并不是先对矩阵做置换然后再统计，因为这种方法太费时，我们可以使用一种方法来“模拟”矩阵置换而得到结果。如上例所示，我们只需要先初始化 Min-Hashing 值为 $\infty$，然后根据 Permutation 以及原矩阵每个元素是否为 1，如果元素为 1 且元素对应的 Permutation value 低于 Min-Hashing 值就更新 Min-Hashing 值。

我们能做上述操作的前提是，Min-Hashing 能保证没有破坏矩阵中原有的列与列之间的相似关系：

$$
\text{similarity of columns == similarity of signatures}
$$

具体证明省略，直觉上来看可以把每次 permutation 然后计算得到的 Min-Hashing 值比作一个人某个角度的拍照，最后得到的 signature 是这个人的多个角度的拍照合集，而这个合集则可以唯一代表这个人，而且没有冗余信息。随着 Permutation 次数越来越多，**Col/Col** 会越来越接近 **Sig/Sig**。

***注意***：

- Min-Hashing 只是适应 Jaccard similarity 的一种 hash 方式，也就是说如果计算距离的方式不是 Jaccard distance 则不能使用 Min-Hashing。

现在来讨论 Min-Hashing 给我们带来的收益: 

- 假设我们进行了 100 次随机 permutation。

- 对于每篇文章，每次 permutation 操作都可以得到一个 Min-Hashing value，因为 term set 已经散列到整数空间上，使用 4 个字节来表示任意一个 term，所以每个 Min-Hashing value 也是 4 个字节，最终每篇文章的 signature 仅为 400 个字节！

# Locality Sensitive Hashing

现在我们已经能够使用非常小的存储空间来表示一篇文章，但是还是没有解决比较时间开销过大的问题，数据量太大我们不可能两两比较。局部敏感哈希（Locality Sensitive Hashing）可以帮助我们解决这个问题。

Locality Sensitive Hashing 的主要思想是：不要对全部数据两两比较，而是从中选择 candidate pairs，再对 candidate pairs 进行确认，如果满足条件则留下，否则剔除。

**如何选择 candidate pairs**。这种选择操作必须得覆盖全部数据，而且需要保证比较高的正确率。

Locality Sensitive Hashing 的方法是将 Min-Hashing 得到的 signatures（向量）“分段“然后进行比较:

1. 设置一个 threshold $s$，如 $s=0.8$，当且仅当两个 signature 的相似度 $\ge s$ 时才被认为满足相似条件。

2. 将 signatures 分为 b 段，每段含有 r 个行。
	
	![chapter3-lsh-partition](/images/mining-massive-datasets/chapter3-lsh-partition.png "Partition M into b bands")

3. 将对应段散列到一个 buckets，当且仅当两个 signature 的任意对应段散列到同一个 bucket 时我们才将其选作 candidate pair。（只要 buckets 的数目足够多，散列到同一个 bucket 就可以视为相同）

	![chapter3-lsh-hashing-bands](/images/mining-massive-datasets/chapter3-lsh-hashing-bands.png "Hashing bands")

有两种选择错误的情况：一是 false positive，被选作 candidate pair 的两列最终验证不满足要求 ，这种情况可以通过后来的验证来解决；二是 false negative，没有被选作 candidate pair 的两列反而满足要求。

Locality Sensitive Hashing 是根据局部过滤数据，所以会同时出现 false positive 和 false negative 两种情况。例如当两列有一个对应段完全相同，然而整体相似度很低，则是 false positive；当两列没有一个对应段完全相同，但整体相似度却满足 threshold，则是 false negative。

现在讨论这个方法的正确性:

- 假设 signature matrix 中的两列的相似度为 $t$；

- 对应 band 相同的概率为 $t^r$；

- 对应 band 不同的概率为 $1-t^r$；

- 任意对应 band 都不同的概率为 $(1-t^r)^b$；

- 存在至少一个对应 band 相同的概率为 $1-(1-t^r)^b$。

假设 $r$ 和 $b$ 固定，则 $p = 1-(1-t^r)^b$ 是关于 $s$ 的 **S 型函数**。

![chapter3-lsh-s-curve](/images/mining-massive-datasets/chapter3-lsh-s-curve.png "r=5, b=10, S curve")

红线代表我们设置的 threshold，蓝色区域表示 false negative rate（满足要求却没能被选中），绿色区域表示 false positive rate（低于 threshold 却被选中）。

有时候可能需要调整 b, r, s 的值来获取最佳结果。




