
### Motivation
客户端的偏差性数据域引发了表征偏见representation bias，这是由于在本地训练过程中特征提取器持续接触有偏的局部数据，从而导致了进一步的退化，即所谓的表征退化representation degeneration。为了解决此问题，本文提出了一种适用于FL的通用框架DBE。
<font color="#ff0000">表征退化和偏见会导致acc下降</font>


### Notation & Preliminaries
写纸上，一知半解


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
模型：对于CV，采用4层CNN即可，并对Tiny额外用了ResNet-18。对于NLP用了fastText
超参数：客户端20，bs=10，局部轮次1，一直跑直到收敛

统计异质场景：
1. Pathological setting： 2/10,20/100,20/200的方式选择标签
2. Practical setting：采用Dirichlet distribution

实验细节：

##### 怎么切割模型？即在何处添加DBE
文中的实验证明在所有FC之前添加效果最好，不过为什么ACC那么低？
Figure-3没看懂


##### Metrics
MDL：越低越好，代表可以用更少的参数表达足够的信息
隐私性：加噪声，借鉴FedPAC

### Metric
#### 介绍部分
tsne图，用来展示特征空间最后会形成一个个cluster
MDL，用来表示标签分类难度



### Learn
+ 表征偏差representation bias，原本聚在一团的表征，会很有区分的成为一个个cluster
+ 最小描述长度minimum description length(MDL)，概念待补。本文中是一种独立于数据和模型的metric，它可以被当作给目标标签进行分类的难度。越大意味着越低的表示质量。


### Question

讲特征提取器和分类器之间的一个表示级别转换为client-specific bias和client-invariant mean
3.1的R没看懂

Figure 3-1没看懂