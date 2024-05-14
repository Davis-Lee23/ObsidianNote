
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
这部分解答了一个读文章时的疑惑，即什么信息是biased，什么信息是unbiased，没有合理的引导


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