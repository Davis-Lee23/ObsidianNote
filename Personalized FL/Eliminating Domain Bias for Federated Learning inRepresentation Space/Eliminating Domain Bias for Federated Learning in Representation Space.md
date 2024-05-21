
### Motivation
客户端的偏差性数据域引发了表征偏见representation bias，这是由于在本地训练过程中特征提取器持续接触有偏的局部数据，从而导致了进一步的退化，即所谓的表征退化representation degeneration。为了解决此问题，本文提出了一种适用于FL的通用框架DBE。
<font color="#ff0000">表征退化和偏见会导致acc下降</font>


### Notation & Preliminaries
纸笔记录


### Method

##### Problem Statement
就是要最小化的损失函数
##### Personalized Representation Bias Memory（PRBM）个性化表征偏差记忆
就是把表征空间Z拆成全局表征Zg和个性化表征Zp。用Zg替代了原本的Zi，并且保存了可训练向量Zp
##### Mean Rgularization 均值正则化
这部分解答了一个读文章时的疑惑，即什么信息是biased，什么信息是unbiased.没有合理的引导特征提取器和可训练PRBM，二者将很难区分什么是biased，什么是unbiased。

正则化了Zg的每一维度均值，然后这个均值会随着batch不断更新

算法：f-特征提取器，h-分类器
最初的Zg由各个客户端训练一次聚合而来，并且客户端保存了自己的Zp
![[Pasted image 20240514164114.png]]


### Experiments
数据集：FMNIST，CIFAR-100，Tiny-ImageNet，AG News
模型：对于CV任务，采用4层CNN即可，但对Tiny用了ResNet-18。对于NLP用了fastText
超参数：客户端20，bs=10，局部轮次1，一直跑直到收敛

统计异质场景：
1. Pathological setting： 2/10,20/100,20/200的方式选择标签
2. Practical setting：采用Dirichlet distribution（比例用β表示）


#### 5.1 添加DBE后的效果
该方法本质上是FedAvg+DBE，这里要在说明加入DBE后的好处
###### 1 怎么切割模型？即在何处添加DBE
文中的实验证明在所有FC之前添加效果最好，不过为什么ACC那么低？
Figure-3没看懂

###### 2 TSNE图
abcd可以理解，ef理解成没有影响吗？

###### 3 消融
MR更好的提升泛化性，PRBM更好的提升准确率

###### 4 隐私保护
参照FedPAC，调整scale parameter和perturbation coeffcient添加高斯噪声。

###### 5 DBE对于传统联邦方法的提升
DBE适用于传统的联邦方法且提升巨大，无论该传统FL属于哪一类的。

#### 对比其它PFL

###### 1 性能比较
对cifar100，参加比例0.5的情况下，DBE也领先于其他方法。在pathological设置下，cifar100相比于FedAvg提升了47.40，领先SOTA11.36%

###### 2 不同异质情况下的能力比较
调整β制造不同的异质场景，在0.01，0.5，5的情况下都是最优。
时间开销处于中上游。

###### 3 给部分PFL添加MR
部分PFL方法方法并不是简单的fedavg，PRBM又会违反部分方法的逻辑，因此仅给一些方法添加MR，可以看到性能有不同程度提升但依然不如Avg+MR+PRBM。

证明PRBM的必要性，MR+PRBM的必要性。

### Conclusion
提出了通用的框架DBE，对准确率和泛化性都有不错的提升。
对于展望的一些看法：
不算问题的问题：下载、上传、聚合仍让会受到攻击，可以指定隐私保护方案
Acc依然还是太低了，Tiny仅仅四十多


##### Metrics
MDL：越低越好，代表可以用更少的参数表达足够的信息。作泛化性指标
隐私性：加噪声，借鉴FedPAC



### Metric
#### 介绍部分
tsne图，用来展示特征空间最后会形成一个个cluster
MDL，用来表示标签分类难度，可作泛化性指标



### Learn
+ 表征偏差representation bias，原本聚在一团的表征，会很有区分的成为一个个cluster
+ 最小描述长度minimum description length(MDL)，概念待补。本文中是一种独立于数据和模型的metric，它可以被当作给目标标签进行分类的难度。越大意味着越低的表示质量。


### Question

讲特征提取器和分类器之间的一个表示级别转换为client-specific bias和client-invariant mean
3.1的R没看懂

Figure 3-1没太看懂