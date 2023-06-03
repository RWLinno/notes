# 使用GCN做节点分类预测

### 实验结果

![image-20230520110603796](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230520110603796.png)

### 总结

- 配置环境花的时间时间大概是最多的，配置好了CUDA、cudnn、Pytorch等，遇到问题比较多（但都是小问题），不过都解决了。
- 看了一些关于GCN的讲解，论文中的公式推导大概有点感觉了，不过还是比较抽象。
- 李沐讲综述的时候提到的一个网址，感觉很有趣https://distill.pub/2021/gnn-intro/



# 图对抗（顺便跑了）

![image-20230521024435420](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230521024435420.png)



# SL-DSGCN

论文：Investigating and Mitigating Degree-Related Biases in Graph Convolutional Networks

来源：2020CIKM

### 概要

- GCN在图的半监督学习上能取得良好表现，但训练难度大
- 当标签数据有限，GCN在**low-degree nodes**的更新信息不足，预测表现不佳
- 提出**SL-DSGCN**(Self-Supervised-Learning Degree-Specific GCN)，从模型和数据上减轻关于度的偏差
- 在数据集中验证了SL-DSGCN提高了自我训练GCN能力和低度节点的准确性。

### 分析问题

- 考虑如何捕捉不同层之间节点的复杂关系 / 如何为低度节点提供信息。

- 使用了 Cora, Citeseer, Pubmed, Reddit 四个数据集，通过可视化分析可以发现预测准确率随着节点度数的变化分布。
- 度数在数据集中并非独立同分布，而度数越大的点往往容易与带标签邻居相邻，可以频繁参与优化过程，这也是GCN在度数大的点性能好的原因之一。

### 实现过程

![image-20230520192919280](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230520192919280.png)

##### Degree-Specific GCN Layer

- SL-DSGCN的核心部分，用来捕捉不同度数节点的差异和相似性
- 传统GCN使用与相邻节点的加权平均来计算特征，该结构将节点的度考虑在内，使度不同的节点在传递消息和更新特征过程有所不同。（也许是特征拼接之后再进行图卷积操作）
- 该层参数由**RNN**生成，大概是因为度不同的节点通过同一个RNN可以获得不同的度的特征信息，从而在性质相似的点中，特征由度来决定。

##### Self-Supervised-Training with Bayesian Teacher Network

- Pre-training with the Annotator

teacher-student Network是用一个更宽更复杂，但已经训练好的神经网络(teacher)去训练另一个窄而深的网络(studen)。这篇论文就是使用这种方法生成**软标签和不确定分数**。

论文里提出带Bayesian teacher network的子监督训练，大概步骤：（1）使用**图注释器**为未标记的节点生成伪标签。

（2）使用生成的伪标签作为额外的监督，在有标签和无标签的数据上训练学生网络。

（3）使用训练好的学生网络，对图中的所有节点进行预测。

（4）在学生网络产生的预测上训练贝叶斯教师网络。

（5）使用训练好的**贝叶斯教师网络**来完善由图注释器生成的伪标签。

（6）重复步骤2-5，直到收敛。

- Fine-tuning Student with Uncertainty Scores

训练好的Bayesian teacher network可以计算每个节点的不确定分数，这东西可以给损失函数加权优化收敛，这样可以更好地利用未标记数据来训练。

##### Training Algorithm

![image-20230520193000543](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230520193000543.png)



### 一些思考

- 所以 graph annotator 要怎么设计得高效快速？按照Paper的说法是一种能够给未标记节点生成伪标签的启发式算法，本质上是也是需要经过训练来提高准确率的，这样的话它应该决定了学生网络的训练效果的阈值了。既然给训练过程分层了，那收敛应该也会变慢，感觉这是一个权衡，毕竟也有可能只对学生网络多迭代几次也能达到同样效果。
- 这篇论文是2020年的，但RNN应该早就可以用更优秀的结构来代替了？比方说LSTM和Transformer啥的。
- 在稠密图的表现如何？感觉准度应该差不了多少但是计算会比较慢吧，所以感觉应该要先判断图的性质再考虑是否进入SL-DSGCN结构。



# Meta-tail2vec

论文：Towards Locality-Aware Meta-Learning of Tail Node Embeddings on Networks

来源：2020CIKM

[GitHub - smufang/meta-tail2vec: Implementation of meta-tail2vec published in CIKM 2020 paper "Towards Locality-Aware Meta-Learning of Tail Node Embeddings on Networks".](https://github.com/smufang/meta-tail2vec)

### 摘要

- 提出meta-tail2vec 方法来优化尾部节点嵌入问题
  - 对于**尾部节点的嵌入**问题，采用重建的回归模型来计算。
  - 对于网络的局部感知位置问题，提出**删边和元学习**的方法局部调整

- 在公共数据集中进行大量实验，验证了meta-tail2vec的性能提升

### 背景

- network embedding
- GNN

### 分析问题

- 对于没有 丰富结构链接/低度数 的节点，嵌入性能较差
- 一节点的度在网络中变化很大，不是独立同分布
- 某些网络的节点度的特点为长尾分布，即相当一部分节点处于尾部且**度数非常低**
- 如何在有限的结构信息中学习有效的**尾部节点的嵌入向量**？？

##### 问题的难点

- 尾部节点提供的信息非常少
- 嵌入时需要考虑每个节点的结构位置，在于无法适应每个节点（链接很少的尾部节点），否则会过拟合。

### 方法

- 对于尾部节点信息稀缺的问题，考虑利用头部节点的嵌入(Oracle embeddings)来训练**回归模型**并且通过**自监督学习重建**(视为一个few-shot问题)。利用该模型，可以预测尾部节点的嵌入使其逼近Oracle embeddings。同时，通过删边操作模拟尾部节点的有限信息。
  - 在邻居随机选k个(**支持集合**)断掉，那样尾部节点嵌入学习就可以变成一个**few-shot**
  - 使支持集合来更新先验知识，再用查询节点更新回归模型
  - 回归模型可以用来预测尾部节点的嵌入向量，由嵌入器和回归其组成
  - 嵌入器将每个节点降维，回归器则将嵌入向量映射到目标值。

- 针对每个节点结构位置问题，采用**元学习**范式，采用模型无关的MAML(Model-Agnostic Meta-Learning)元学习框架。
  - 通过在多个任务上进行梯度下降来学习通用初始化参数，使这些参数快速适应新任务
  - 更新后的参数在查询集上预测从而得到嵌入向量。
- 实验是在3个公共数据集上做节点分类和链路预测任务

### 一些思考

- 这篇论文陌生用的方法好像有点多，有点看不过来，像文中提到的SDNE、ARGA、DDGCN等一系列嵌入方法都没来得及细究，不过大致就是元尾部嵌入要比这些都好就是了
- 关于few-shot的部分有点像直接用了掩码的思想，总之就是把邻居mask掉去训练。虽然文章的方法是把**询问节点**的k个邻居mask掉，但我觉得直接在整个图上按度数贡献到概率上再随机mask也是可以的。
- 感觉这个方法会很慢，举个例子我有一个菊花图，中心是A。首先一个问题是其他全是尾部节点，A永远在支持集合中，给的权重太高了；其次是如果不自适应跳数的话，实验中2跳的性能阈值就会很慢很慢，因为这个情况每个点的信息都是整张图所有节点给的。



# TNGNN

论文：Tail-GNN：Tail-Node Graph Neural Networks

来源：2021KDD

### 摘要

- 针对长尾效应提出**Tail-GNN**模型
- 提出**transferable neighborhood translation**的概念，捕获节点与邻居的传递关系
- 在五个公共数据集上进行实验，Tail-GNN取得了SOTA成绩

### 背景

- 图数据集的普遍特征：长尾分布
- 最近的GNN模型虽然很好地学习了节点表示，但是不能针对处理尾部节点
- 尾部节点的结构信息缺失影响节点分类和链路预测等任务

### 分析问题

- 如何发现尾节点缺失的邻域信息？
  - Tail-GNN uses a novel concept of transferable neighborhood translation to model the variable ties between a target node and its neighbors. This allows it to predict missing neighborhood information for tail nodes and complement their neighborhood aggregation.
- 如何在保持节点间通用性的同时，对每个尾节点的缺失信息进行局部定位？
  - Tail-GNN uses a transferable form of neighborhood translation that can be further localized to suit the local context of each node. This allows Tail-GNN to maintain generality across nodes while still being able to adapt to local contexts and provide accurate predictions for missing neighborhood information.

### 方法

##### Neighborhood translation

- contrastive strategy
- localizing strategy

##### neighborhood aggregation

- for head nodes: directly aggregate over the observed neighborhood
- for tail nodes: $h_v^{l+1}=\mathcal M(h_v^l,\{m_v^l\}\cup {h_i^l : i\in \mathcal{N_v};\theta^{l+1}} )$

##### Overall loss

- task loss
- loss for missing information constraint
- loss for adversarial constraint

$$
Overall\quad loss:Min_{\Theta}(Max_{\theta_d}(\mathcal L_t +\mu\mathcal L_m -\eta\mathcal L_d))\\
$$

### 一些思考

- 消融实验好像没有讨论到超参数对Tail-GNN的影响，只有某一部分方法缺失的影响，内部的参数到底是怎么定义的？
- 随机缺失信息应该会导致每一次的准确率都不太一样吧，这是怎么做的比较实验？
- 这个模型是包含了对比学习？或者说是不是可以用对比学习来优化调参？
