# Attention

论文名：Recurrent Models of Visual Attention

时间：*[Submitted on 24 Jun 2014]*

其实很早就提出了这个思想，但是在这篇论文后火的。

### 要点

- 在RNN模型上使用attention机制来进行图像分类，从而进军CV领域，随后进入NLP领域。



# Transformer

论文名：Attention Is All You Need

时间：*[Submitted on 12 Jun 2017 (*[v1](https://arxiv.org/abs/1706.03762v1)*), last revised 6 Dec 2017 (this version, v5)]*



# Bert

论文名：BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding

时间：*[Submitted on 11 Oct 2018 (*[v1](https://arxiv.org/abs/1810.04805v1)*), last revised 24 May 2019 (this version, v2)]*



# MoCo

论文名：Momentum Contrast for Unsupervised Visual Representation Learning

时间：*[Submitted on 13 Nov 2019 (*[v1](https://arxiv.org/abs/1911.05722v1)*), last revised 23 Mar 2020 (this version, v3)]*

### 要点

- 把InsDis里的Memory Bank换成了队列，并且加入了动量编码器。



# SimCLR

论文名：A Simple Framework for Contrastive Learning of Visual Representations

时间：*[Submitted on 13 Feb 2020 (*[v1](https://arxiv.org/abs/2002.05709v1)*), last revised 1 Jul 2020 (this version, v3)]*

### 要点



# BYOL

论文名：Bootstrap your own latent: A new approach to self-supervised Learning

时间：*[Submitted on 13 Jun 2020 (*[v1](https://arxiv.org/abs/2006.07733v1)*), last revised 10 Sep 2020 (this version, v3)]*

### 要点

- 去掉了负样本，自己跟自己学，把对比任务变成了预测任务。
- 目标函数不再使用info NCE,而是用MSE Loss
- 因为BN的归一化操作被质疑存在负样本，随后使用了别的归一化打破了这个质疑



# SimSiam

论文名：Exploring Simple Siamese Representation Learning

时间：*[Submitted on 20 Nov 2020]*

### 要点

- 化繁为简，提出孪生网络，总结了前人堆叠的工作。
- 因为用stop gradient避免坍塌，可以看成是EM的一种。



# Vision Transformer

论文名：An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale

时间：*[Submitted on 22 Oct 2020 (*[v1](https://arxiv.org/abs/2010.11929v1)*), last revised 3 Jun 2021 (this version, v2)]*

### 要点

- 把图片割成$16×16$个patch，每个方块做成词，放入Transformer进行训练。 



### 一些想法

2023.2.9

**关于patch的划分** - 上周组会没有提前了解，读了论文发现里面的patch的划分确实可以取到$\sqrt D$的，或者说是接近这个值的整数。文章提到的D便是embedding的维度(因为X,Y轴都只需要$\frac{D}{2}$的位就能表示$2^{\frac{D}{2}}$d的信息)

**关于patch序列是否能随机** - 文章用的是BERT’s class token和1D position embeddings，但似乎提到了位置编码这一部分是无所谓的，在swin里我感觉窗口确实也可以多次随机映射到哈希序列去进行训练，后面读了MAE的论文，也重新提到了这个点。



# Swin-Transformer

论文名：Swin Transformer: Hierarchical Vision Transformer using Shifted Windows

时间：*[Submitted on 25 Mar 2021 (*[v1](https://arxiv.org/abs/2103.14030v1)*), last revised 17 Aug 2021 (this version, v2)]*

### 要点

- 开源，有各个大小版本，可灵活运用于分类、分割、检测等任务中

### 一些想法

**关于为什么要划分9个形状不一的窗口** - Swin-Transformer中对窗口的划分是可以用随机哈希的吗？能不能找到一个窗口划分数的最佳值。（原论文里划分了9个形状不一的窗口并且按规律成对拼接）



# Video Swin Transformer

论文名：Video Swin Transformer

时间：*[Submitted on 24 Jun 2021]*  From: Yue Cao

### 要点

- 结构与Swin Transformer非常接近，运用到视频领域，在模型计算时多了时间维度



# MAE

论文名：Masked Autoencoders Are Scalable Vision Learners

时间：*[Submitted on 11 Nov 2021 (*[v1](https://arxiv.org/abs/2111.06377v1)*), last revised 19 Dec 2021 (this version, v3)]*



### 要点

- NLP中的BERT和Transformer分别对应CV领域的MAE和ViT。



### 一些想法

**关于随机patch序列的编码解码** - 这篇文章在Simple implementation那里提及了随机打乱patch序列取前25%进行编码，后面的75%进行mask，然后不被打乱的整个序列去用transformer解码。看到这里感觉自己之前想象力不够，这个思路确实很容易被想到，因为窗口是在patch之上的又一层，所以现在比较担心窗口的打乱是否多此一举，或者会不会让滑动窗口失去优势。



# ShiftViT

论文名：When Shift Operation Meets Vision Transformer: An Extremely Simple Alternative to Attention Mechanism

时间：*[Submitted on 26 Jan 2022]*  accepted by AAAI-22



### 要点

- 在ViT中，attention可以用位移操作来代替。
- 提出的结构相当于把Swin-transformer中的attention架空，可以让模型效率更高



### 一些想法

Vision Swin Transformer想必也可以同样进行这种修改？？



# LSTCL

论文名：Long-Short Temporal Contrastive Learning of Video Transformers

时间：*[Submitted on 17 Jun 2021 (*[v1](https://arxiv.org/abs/2106.09212v1)*), last revised 31 Mar 2022 (this version, v3)]*



### 要点

- 用LSTCL代替基于监督的图像预训练
- 由于Transformer可以有效捕捉在时间跨度的依赖关系，因此令模型将同一视频的长短期视图匹配
- 比较了运用到不同学习框架上的性能



# VideoMAE

论文名：VideoMAE: Masked Autoencoders are Data-Efficient Learners for Self-Supervised Video Pre-Training

时间：*[Submitted on 23 Mar 2022 (*[v1](https://arxiv.org/abs/2203.12602v1)*), last revised 18 Oct 2022 (this version, v3)]*

### 要点

- 提出简单高效的自监督学习方法Video-MAE，用于Video-Transformer预训练
- 用极高的掩蔽率( e.g. 90%-95% ) 和Pipe掩蔽策略，鼓励VideoMAE学习更多有代表性特征并缓解信息泄露问题
- 与ViViT/Motionformer/Video Swin等方法在ImageNet、Kinetics的各种数据集上进行了大量实验比较
